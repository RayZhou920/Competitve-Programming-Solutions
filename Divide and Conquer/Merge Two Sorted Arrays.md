Merge Two Sorted Arrays：合并排序数组。
===========

![image](https://user-images.githubusercontent.com/91653378/137281943-d3a7bcfe-4d03-4248-b675-26486a0c25d4.png)

// Solution 2: Recursion
--------
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        // base case;
        if (l1 == null) {
            return l2;
        }
        if (l2 == null) {
            return l1;
        }
        // 假如l1第一个节点小于l2，将l1第二个节点与l2第一个节点比，以此类推，所以l1.next = mergeTwoLists(l1.next, l2);
        if (l1.val <= l2.val) {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        }
        // 假如l2第一个节点小于l1，将l2第二个节点与l1第一个节点比，以此类推，所以l2.next = mergeTwoLists(l1, l2.next);
        else {
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }
    }
}
