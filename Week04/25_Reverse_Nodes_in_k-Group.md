# 25. Reverse Nodes in k-Group

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {

        if (!head || k==1 ) return head;

        ListNode dummy(0);
        dummy.next = head;

        ListNode* prevGroupTail = &dummy;

        while(true){
            ListNode* kth = prevGroupTail;

            for (int i=0; i<k && kth; i++){     //find the kth node of this group
                kth = kth->next;
            }

            if(!kth) break; // if it remains < k, just return

            ListNode* groupStart = prevGroupTail-> next;
            ListNode* nextGroupHead = kth->next;

            ListNode* reverseNext = nextGroupHead;
            ListNode* curr = groupStart;

            while(curr != nextGroupHead){
                ListNode* temp = curr->next; // record the next node to process
                curr->next = reverseNext;      // set the next node of curr after reversing
                reverseNext = curr;     // the next node of the node to process next will be curr
                curr = temp;    // move to next node
            }

            prevGroupTail->next = kth;
            prevGroupTail = groupStart;
        }

        return dummy.next;
    }
};
```