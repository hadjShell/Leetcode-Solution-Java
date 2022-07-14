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

