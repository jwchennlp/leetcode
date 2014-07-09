##问题

有一排序好的数组并在数组的某一未知位置进行了旋转(如[0,1,2,3,4,5,6,7]旋转为[5,6,7,0,1,2,3,4]).			

给定一个目标值查找值是否出现在数组中,如果出现则返回在数组中的下表,否则返回-1.		

##想法		

###1.简单 	

翻转后数组其实变成了两个递归上升的子数组,所以我们需要找到数组的最大值所在的索引位置.而后对两个子数组进行遍历查找.		

如A=[4,6,1,2,3],tar=5.

先遍历数组找到最大值6的下表1,则有两个子数组A[0,...,1]和A[2,...,4],且左边数组更大.下面对数组进行遍历求解.		

可以知道,查找数组最大值下标的时间复杂度为o(n),遍历两个子数组的时间复杂度也为O(n),所以整体的时间复杂度为O(n).


###2.进阶  		

在查找最大元素下标和判断目标值的过程中都是遍历实现,我们可以通过二分查找的方式提高效率.时间复杂度log(n).		

```C++		
 findpivot(int A[],int low,int high){
        if(low>high)
            return -1;
        if(low==high)
            return low;
        int mid=(low+high)/2;
        if(A[mid]>A[mid+1]&&mid<high)
            return mid;
        
        if(A[low]>A[mid]){
            findpivot(A,low,mid-1);
        }else
            findpivot(A,mid+1,high);
    }
    
    int findtar(int A[],int low,int high,int target){
        if(low>high)
            return -1;
        int mid = (low+high)/2;
        if(A[mid]==target)
            return mid;
        else if(A[mid]>target)
            findtar(A,low,mid-1,target);
        else
            findtar(A,mid+1,high,target);
    }
    
    int search(int A[], int n, int target) {
        int pivot = findpivot(A,0,n-1);
        if(pivot==-1)
            return findtar(A,0,n-1,target);
        if(A[pivot]==target)
            return pivot;
        else if(target>=A[0])
            findtar(A,0,pivot-1,target);
        else
            findtar(A,pivot+1,n-1,target);
    }
```		

在上面方法中,我们仍需要通过二分查找找出最大元素的下标,我们可以再简化一下,之需要判断数组的中位数和最左边数的关系,如果A[low]>A[mid],则说明A[low...mid]是先增后减,A[mid...high]则是递增.如果A[low]<A[mid],则A[low...mid]是递增.			

这样每次进行折半查找,时间复杂度为O(lgn).


```C++
    int findtar(int A[],int low,int high,int target){
        if(low>high)
            return -1;
        if(low==high){
            if(A[low]==target)
                return low;
            return -1;
        }
        int mid = (low+high)/2;
        if(A[mid]==target)
            return mid;
        if(A[low]>A[mid]){
            if(target>A[mid]&&target<=A[high])
                findtar(A,mid+1,high,target);
            else
                findtar(A,low,mid-1,target);
        }
        else{
            if(target<A[mid]&&target>=A[low])
                findtar(A,low,mid-1,target);
            else 
                findtar(A,mid+1,high,target);
        }
    }
    
    int search(int A[], int n, int target) {
        return findtar(A,0,n-1,target);
    }
```	