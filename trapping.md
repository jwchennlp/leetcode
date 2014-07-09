##问题

给定n个非负整数代表柱形图,其中每一栏的宽度为1,需要计算整个柱形图能贮存多少水.		

例如 给定[0,1,0,2,1,0,1,3,2,1,2,1],返回6.		

![img](./img/1.png)		


##思路		

对于数组中的每一个元素,其值为柱形图对应栏的高度,在此栏能贮存多少水取决于其左侧和右侧的最高栏的高度.		

即如果min(left(A,i),right(A,i))>A[i],则vol[i]=min(left(A,i),right(A,i))-A[i].否则vol[i]=0.		

所以最简单的方法是创建空间分别从左至右和从右至左遍历数组,求出某一点的左侧最大值left[i]和右侧最大值right[i],并计算得出结果.		

这里空间复杂度为O(n),时间复杂度为O(n).		



###优化		

可以不用额外的空间解决问题,我们需要left和right下标.当访问i元素时,left代表A[0...i-1]中的最大值,right代表A[i...n-1]中的最大值.		

而后计算i点的储水量以及A[0...i]的left值和A[i+1...n]的left值.		

```C++	
    int cal(int A[],int i,int n,int &left,int &right){
        if(i<right){
            if(A[i]>=A[left]){
                left=i;
                return 0;
            }
            else{
                if(A[i]<A[right])
                    return min(A[left],A[right])-A[i];
                else
                    return 0;
            }
        }
        else{
            left=right;
            right=rightmax(A,i+1,n);
            return 0;
        }
        
    }
    int rightmax(int A[],int i,int n){
        int tmp=A[i],tag=i;
        for(int j=i+1;j<=n-1;++j){
            if(A[j]>tmp){
                tmp = A[j];
                tag=j;
            }
        }
        return tag;
    }
    int min(int a,int b){
        int res=a<b?a:b;
        return res;
    }
    int trap(int A[], int n) {
        if(n<=2)
            return 0;
        int vol=0;
        int left=0;
        int right = rightmax(A,2,n);
        for(int i=1;i<n-1;++i){
            vol += cal(A,i,n,left,right);
        }
        return vol;
    }	
```		

其中可以发现,在每次i到达右侧的最大值处时,需要从新更新右侧的最大值.所以如果数组是递减程序排序的话,则需要o(n^2)时间来计算right下标.		


###更简洁的方法		

找到到一种更巧妙的方法,从数组的两侧向中间遍历,curlevel用于存储当前遍历中最高的元素值.具体实现见代码.		

```C++
    int min(int a,int b){
        int res=a<b?a:b;
        return res;
    }
    int trap(int A[], int n) {
        int l=0,r=n-1,curlevel=0,block=0,all=0;
        while(l<=r){
            if(min(A[l],A[r])>curlevel){
                all += (min(A[l],A[r])-curlevel)*(r-l+1);
                curlevel = min(A[l],A[r]);
            }
            if(A[l]<A[r])
                block += A[l++];
            else
                block += A[r--];
        }
        return all-block;
    }
```		

只需要遍历一次数组,并且不需要额外开辟空间.所以时间复杂度为O(n).		

