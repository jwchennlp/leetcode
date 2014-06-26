##问题	

给定一已排序数组,去除重复的元素使得每一元素只出现一次并返回新数组的长度.		

**限制:**不能申请而外的内存空间.		

例如,A=[1,1,2],则返回length=2,并且A=[1,2].		

##想法		

思想很简单.首先我们对第一个数据做一个标记(tag),设置length=1,从下个元素开始遍历,如果元素与标记不同,length+1,将标记设为当前元素.		
如果下一元素与标记相同,则需要一直往后遍历直到找到一个不相同的元素num或是数组尾部,将num放到A[length]中,length+1,tag=num.直到遍历到数组尾.		

例如A=[1,1,1,2,4,5,5].		

初始tag=1,length=1,从A[1]开始遍历,A[1]等于tag,一直往后遍历找到A[3]不等于tag,则置A[length]=A[3],tag=A[3].A=[1,2,1,2,4,5,5].	

tag=2,length=2,从A[4]开始便利,A[4]不等于tag,tag=A[4],A[length]=A[4],length++,A=[1,2,4,2,4,5,5].		

tag=4,length=3,从A[5]开始遍历,A[5]不等于tag,tag=A[5],....,A=[1,2,4,5,4,5,5].		

tag=5,length=4,从A[6]开始便利,A[6]等于tag,往后遍历至数组尾未找到新元素,结束.		

```C++		
    int removeDuplicates(int A[], int n) {
        if(n==1||n==0)
            return n;
        int begin=1,i=1;
        int tag=A[0];
        while(i!=n){
            if(A[i]!=tag){
                A[begin++]=A[i];
                tag=A[i];
            }
            else{
                for(int j=i+1;j!=n;++j){
                    if(A[j]!=tag){
                        A[begin++]=A[j];
                        tag=A[j];
                        i=j;
                        break;
                    }
                }
            }
            ++i;
        }
        return begin;
    }
```		

需要遍历一次数组,时间复杂度为O(n).

###更简单的方法

其实,在往前遍历数组的时候,我们需要不断的将未访问过的元素往前提,重复的元素之考虑一次,通过维护front和back两个下标便能实现优化.front用于保留未重复的元素长度,back表示便利的数组的下表.		

```C++
    int removeDuplicates(int A[], int n) {
        if(n==0||n==1)
            return n;
        int front=1,back=1;
        for(;back!=n;++back){
            if(A[back]!=A[front-1])
                A[front++]=A[back];
        }
        return front;
        
    }
```
需要遍历一次数组,时间复杂度为O(n).
