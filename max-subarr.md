##问题

对某一数组,找出数组的某一连续子数组,使的子数组的元素综合最大.		
例如:数组[−2,1,−3,4,−1,2,1,−5,4],			
则连续子数组[4,−1,2,1]的子数组和最大,sum=6.		

限制:先o(n)的时间复杂度内求得答案.		

##思路		

其实可以知道,最大和子数组的第一位元素一定为正数(反证法可证明).		

```c++						
int maxSubArray(int A[], int n) {		
        int max = Integer.MIN_VALUE;		
        int current  = 0;		
        for(int i=0;i!=n;++i){		
            current += A[i];		
            max = max>current?max:current;		
            if (current<0)		
                current = 0;		
        }		
        return max;		
    }		
```		

代码只需要一遍遍历数组便能求出最优解.实现的核心部分是引入了变量current.我们发现在计算过程中如果current<0,则需要将current置为0.这么做的原因就是上面提到的,最大子数组的第一位一定为正数.如果current为0,则表示数组的某一序列x的元素和小于0,则显然最有数组起始部分不会出现在x中(同样用反证法可证明).

如数组[1,−3,4,−1,2,1,−5,4]

当x=1时,current=1,大于0,max=1;
当x=1,-3时,current=-2,小于0,max=1;此时,我们知道最优序列肯定不会以序列1,-3开始.所以设置current=0,从一下元素重新考虑.		


##分治法求解		

数组左右边界分别为low,high.中间边界为middle=(low+high)/2.		
则数组的最大子数组可以分为下面三种情况:		
(1). 最大子数组出现在A[low,middle-1]中.		
(2). 最大子数组出现在A[middle+1,high]中.		
(3). 最大子数组包含中间元素A[middle].		

实现代码如下:

```c++			
int divide(int A[],int low,int high){		
        if(low==high)		
            return A[low];		
        if(low==high-1)		
            return max(A[low]+A[high],max(A[low],A[high]));			
        int middle = (low+high)/2;			
        int lmax = divide(A,low,middle-1);		
        int rmax = divide(A,middle+1,high);		
        int mmax = A[middle];		
        int tmp =mmax;		
        for(int i =middle-1;i>=low;--i){		
            tmp += A[i];		
            if(tmp>mmax)		
                mmax = tmp;		
        }		
        tmp = mmax;		
        for(int i=middle+1;i<=high;++i){		
            tmp += A[i];		
            if(tmp>mmax)		
                mmax = tmp;		
        }		
        return max(mmax,max(rmax,lmax));		
    }		
    int maxSubArray(int A[], int n) {		
        return divide(A,0,n-1);				
    }			
```					

问题求解可以表述为:T(n)=2T(n/2)+o(n),时间复杂度为o(nlogn).	

