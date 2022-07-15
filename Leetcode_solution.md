# Leetcode Solution

## Template

## Question

### Description

### Example

### Solution

***

## Question 2

### Description

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

### Example

```markdown
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]

Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]

Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
```

### Solution

* Usage of singly linked list
* Consider **carry**, and the method used for calculating it

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode cur = l2;
        ListNode sum = cur;
        int carry = 0;
        ListNode prev = null;
        
        while(l1 != null && cur != null) {
            cur.val = l1.val + cur.val + carry;
            carry = cur.val / 10;
            cur.val %= 10;
            
            l1 = l1.next;
            prev = cur;
            cur = cur.next;
        }
        
        /* if l1 go to the end, go with cur, do nothing
        ** if cur go to the end, link prev node to l1 current node
        */
        if(cur == null) {
            prev.next = l1;
            cur = l1;
        } 
        
        while(cur != null) {
            cur.val += carry;
            carry = cur.val / 10;
            cur.val %= 10;
            
            if(carry == 0)  break;

            prev = cur;
            cur = cur.next;
        }
        
        // cur go the end, check carry, if 1, create a new node
        if(carry == 1) {
            prev.next = new ListNode(1, null);
        }
        
        return sum;
    }
}
```



***

## Question 9 

### Description

Give an integer `x`, return `true` if `x` is palindrome integer.

### Example

```markdown
Input: x = 121
Output: true

Input: x = -121
Output: false

Input: x = 10
Output: false
```

### Solution

* Save time by operating on half of the number

```java
class Solution {
    public boolean isPalindrome(int x) {
        if(x < 0)
            return false;
        
        if(x < 10)
            return true;
        
        // Special case: end with 0, couldn't get the last half properly
        if(x % 10 == 0)
            return false;
        
        // reversed last half part of x, save time for reversing the whole number
        int lastHalf = 0;
        while(x > lastHalf) {
            lastHalf = lastHalf * 10 + x % 10;
            x /= 10;
        }
        
        return x == lastHalf || x == lastHalf / 10;  
    }
}
```

***

## Question 19

### Description

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

![img](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

- The number of nodes in the list is `sz`.
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

### Example

```markdown
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]

Input: head = [1], n = 1
Output: []

Input: head = [1,2], n = 1
Output: [1]
```

### Solution

* Quick slow pointers

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if(head == null || head.next == null)
            return null;
        
        // create a sentinel
        ListNode sen = new ListNode(0, head);
        
        // slow is the deleted node
        ListNode slow = head, quick = head, preSlow = sen;
        while(quick != null) {
            if(n <= 0) {
                preSlow = slow;
                slow = slow.next;
            }
            
            quick = quick.next;
            n--;
        }
        
        preSlow.next = slow.next;
        return sen.next;
    }   
}
```

***

## Question 21

### Description

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists in a one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return *the head of the merged linked list*.

### Example

![img](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)

### Solution

* Recursion

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1 == null)
            return l2;
        if(l2 == null)
            return l1;
        
        if(l1.val >= l2.val) {
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }
        else {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        }
    }
}
```

