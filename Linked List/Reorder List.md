**Question**: [Reorder List](https://leetcode.com/problems/reorder-list/)

**Approach**:
- Use slow and fast pointers to find the middle of the list.
- Split the list at the middle, then reverse the second half in place.
- Merge the two halves by alternating nodes: first from the original front half, then from the reversed second half.
- All operations relink existing nodes, so no additional list is created.

TC: `O(n)`, since each node is traversed a constant number of times.
SC: `O(1)` auxiliary space.

**Code**:
```cpp
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
    void reorderList(ListNode* head) {
        // Find the midpoint; fast advances twice as quickly as slow.
        ListNode* slow = head, *fast = head;
        while(fast != NULL && fast->next != NULL){
            slow = slow->next;
            fast = fast->next->next;
        }

        // Split after slow so the first half ends here.
        ListNode* nextHalf = slow->next;
        slow->next = NULL;

        // Reverse the second half; prev becomes its new head.
        ListNode* nxt = NULL, *prev = NULL, *curr = nextHalf;
        while(curr != NULL){
            nxt = curr->next;
            curr->next = prev;
            prev = curr;
            curr = nxt;
        }

        // Interleave nodes from the first half and the reversed second half.
        ListNode* first = head, *second = prev;
        while(second){
            // Save both next nodes before changing either link.
            ListNode* temp1 = first->next, *temp2 = second->next;
            first->next = second;
            second->next = temp1;
            first = temp1;
            second = temp2;
        }
    }
};
```