##问题		

给定一个链表，交换链表中每两个相邻的节点，并返回链表的头节点．			

例如：给定１->2->3->4,返回链表为2->1->4->3.			

**限制：**算法只能使用常量空间，不能修改链表中节点的值，只能交换节点．		

##想法	

###循环实现	

从链表头开始便利，如果存在两个非空节点，则进行节点更换，知道遍历到节点末尾．			

在进行节点交换时，我们需要当前节点的前部(pre)，当前节点(cur)和下一节点(n)．	

则:		
pre->next = n;		
cur->next = n->next;
p->next->next =cur;

可是在初始遍历链表时，不知道头节点的前部，所以链表的最开始的两个节点应该单独交换．这样显然会复杂很多．当然，我们可以先随意新建一个节点（node），将节点指向链表．这样，pre=node,cur=head,n=cur->next.这样就可以循环实现节点交换．		

```C++		
    ListNode *swap(ListNode *pre,ListNode *p,ListNode *n){
        ListNode *tmp=n->next;
        pre->next=n;
        n->next=p;
        p->next=tmp;
        return n;
    }
    ListNode *swapPairs(ListNode *head) {
        if(head==NULL)
            return NULL;
        if(head->next==NULL)
            return head;
        ListNode *start = new ListNode(0);
        start->next=head;
        ListNode *cur = start;
        while(cur->next!=NULL&&cur->next->next!=NULL){
            cur->next = swap(cur,cur->next,cur->next->next);
            cur = cur->next->next;
        }
        return start->next;
        
    }
```	

通过链表的一次遍历完成求解，时间复杂度为O(n).

###递归实现		

也可以用递归来求解此问题，在遍历链表的过程中，交换两个节点后，剩余的部分作为子问题递归的求解完成．		

```C++		
    ListNode *swapPairs(ListNode *head){
        if(head==NULL||head->next==NULL)
            return head;
        ListNode *node = new ListNode(0);
        ListNode *start=node;
        node->next =head;
        node->next=head->next;
        head->next = swapPairs(head->next->next);
        node->next->next=head;
        return start->next;
    } 	
```
