# 24. Swap Nodes in Pairs

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
    ListNode* swapPairs(ListNode* head) {
        if (!head || !head->next) return head; //if empty list or only one node

        ListNode* prev = nullptr;
        ListNode* change1 = head;
        ListNode* change2 = head->next;

        head = change2;

        while(true){
            if(prev){
                prev->next = change2;       // make prev node point at the second node of the pair
            }
            change1->next = change2->next;  // swap the pair node
            change2->next = change1;

            // update the pointer
            prev = change1;
            change1 = change1->next;
            if (!change1) break;        // if there's no next pair, break the loop
            change2 = change1->next;
            if (!change2) break;        // if there's odd nodes, there will remain one node for the last pair, break the loop
        }

        return head;        
    }
};
```