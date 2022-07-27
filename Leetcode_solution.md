# Leetcode Solution

## Template

## Question

### Description

### Example

### Solution

***

## Question 2

*Add Two Numbers*

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
```

***

## Question 9 

*Palindrome Number*

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
```

***

## Question 19

*Remove Nth Node From End of List*

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
```

***

## :star:Question 20

*Valid Parentheses*

### Description

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

### Example

```
Input: s = "{ { } [ ] [ [ [ ] ] ] }"
Output: true
Input: s = "([)]"
Output: false
```

### Solution

* If meet left parenthesis push correspond right one into the **stack**. If meet right parenthesis, if the peek of stack is itself, pop the stack and continue, otherwise it indicates that there is a cross of different parentheses in the string
* Check the size of the stack in the end, if it is not equal to 0, there is at least one parenthesis unpaired in the string

```java
public boolean isValid(String s) {
    if(s.length() % 2 == 1)
        return false;

    char[] stack = new char[s.length()];
    int head = 0;

    for(int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        switch(c) {
            // add correspond parenthesis to r
            case '(':
                stack[head++] = ')';
                break;
            case '{':
                stack[head++] = '}';
                break;
            case '[':
                stack[head++] = ']';
                break;
            default:
                // condition and operation at the same time
                if(head == 0 || stack[--head] != c)
                    return false;
                break;
        }
    }
    return head == 0;
}
```

***

## :star:Question 21 

*Merge Two Sorted Lists*

### Description

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists in a one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return *the head of the merged linked list*.

### Example

![img](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)

### Solution

* Recursion

```java
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
```

***

## Question 24

*Swap Nodes in Pairs*

### Description

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

### Example

![img](https://assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg)

```markdown
Input: head = [1,2,3,4]
Output: [2,1,4,3]
```

### Solution

```java
public ListNode swapPairs(ListNode head) {
    if(head == null || head.next == null)
        return head;

    ListNode sentinel = new ListNode(0, head);
    ListNode cur = head, next = head.next, prev = sentinel;
    head = next;
    while(cur != null && cur.next != null) {
        next = cur.next;
        cur.next = next.next;
        next.next = cur;
        prev.next = next;
        prev = cur;
        cur = cur.next;
    }
    return head;
}
```

***

## Question 61

*Rotate List*

### Description

Given the `head` of a linked list, rotate the list to the right by `k` places.

- The number of nodes in the list is in the range `[0, 500]`.
- `-100 <= Node.val <= 100`
- `0 <= k <= 2 * 109`

### Example

![img](https://assets.leetcode.com/uploads/2020/11/13/rotate1.jpg)

![img](https://assets.leetcode.com/uploads/2020/11/13/roate2.jpg)

### Solution

* Quick, slow pointer

```java
public ListNode rotateRight(ListNode head, int k) {
    if(head == null || head.next == null)
        return head;

    ListNode cur = head;
    int size = 0;
    while(cur != null) {
        size++;
        cur = cur.next;
    }

    int rotate = k % size;
    if(rotate == 0)
        return head;

    ListNode quick = head, tail = null;
    cur = head;
    while(quick != null) {
        if(rotate < 0) {
            cur = cur.next;
        }
        rotate--;
        tail = quick;
        quick = quick.next;
    }
    tail.next = head;
    head = cur.next;
    cur.next = null;
    return head; 
}
```

***

## Question 82

*Remove Duplicates from Sorted List II*

### Description

Given the `head` of a sorted linked list, *delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list*. Return *the linked list **sorted** as well*.

- The number of nodes in the list is in the range `[0, 300]`.
- `-100 <= Node.val <= 100`
- The list is guaranteed to be **sorted** in ascending order.

### Example

```markdown
Input: head = [1,2,3,3,4,4,5]
Output: [1,2,5]
```

### Solution

```java
// Solution 1
// Use three pointers, follow the logic
public ListNode deleteDuplicates(ListNode head) {
        if(head == null || head.next == null)
            return head;
        
        ListNode sentinel = new ListNode(-101, head);
        ListNode pSen = new ListNode(-102, sentinel);
        ListNode prev = sentinel, cur = head, pPrev = pSen;
        boolean deleted = false;
        while(cur != null) {
            if(cur.val == prev.val) {
                pPrev.next = cur.next;
                deleted = true;
            }
            else {
                // if there is no node deleted before current node, move pPrev pointer
                // otherwise no need to move it
                if(!deleted)    pPrev = prev;
                prev = cur;
                deleted = false;
            }
            cur = cur.next;
        }
        
        return sentinel.next;
}

// Solution 2
// loop all next nodes of current next node which have same value with it
// current node always has different value with current next node
public ListNode deleteDuplicates(ListNode head) {
    if(head == null || head.next == null) return head;

    ListNode sentinel = new ListNode(-101, head);
    ListNode curr = sentinel;

    while(curr.next != null && curr.next.next != null){
        if(curr.next.val == curr.next.next.val){
            int val = curr.next.val;
            while(curr.next != null && curr.next.val == val){
                curr.next = curr.next.next;
            }
        }else{
            curr = curr.next;
        }
    }
    return sentinel.next;
}
```

***

## Question 83

*Remove Duplicates from Sorted List*

### Description

Given the `head` of a sorted linked list, *delete all duplicates such that each element appears only once*. Return *the linked list **sorted** as well*.

### Example

```markdown
Input: head = [1,1,2,3,3]
Output: [1,2,3]
```

### Solution

```java
public ListNode deleteDuplicates(ListNode head) {
    if(head == null || head.next == null)
        return head;

    ListNode cur = head.next, prev = head;
    while(cur != null) {
        if(cur.val == prev.val) {
            prev.next = cur.next;
        }
        else {
            prev = cur;
        }
        cur = cur.next;
    }

    return head;
}
```

***

## Question 86

*Partition List*

### Description

Given the `head` of a linked list and a value `x`, partition it such that all nodes **less than** `x` come before nodes **greater than or equal** to `x`.

You should **preserve** the original relative order of the nodes in each of the two partitions.

- The number of nodes in the list is in the range `[0, 200]`.
- `-100 <= Node.val <= 100`
- `-200 <= x <= 200`

### Example

![img](https://assets.leetcode.com/uploads/2021/01/04/partition.jpg)

```markdown
Input: head = [1,4,3,2,5,2], x = 3
Output: [1,2,2,4,3,5]
```

### Solution

* Note that when `head.val < x` there are two different scenarios, **the only difference is whether to alter the `prev` pointer or not**

```java
// hold last node pointer whose value is less than x 
public ListNode partition(ListNode head, int x) {
    if(head == null || head.next == null)
        return head;

    ListNode sentinel = new ListNode(-201, head);
    ListNode less = sentinel, prev = sentinel;
    while(head != null) {
        if(head.val < x) {
            // if prev node is last less node, no need to move
            if(prev == less) {
                prev = head;
            }
            else {
                prev.next = head.next;
                head.next = less.next;
                less.next = head;
            }
            less = less.next;
        }
        else {
            prev = head;
        }
        head = prev.next;
    }

    return sentinel.next;
}
```

***

## Question 92

*Reverse Linked List II*

### Description

Given the `head` of a singly linked list and two integers `left` and `right` where `left <= right`, reverse the nodes of the list from position `left` to position `right`, and return *the reversed list*.

### Example

![img](https://assets.leetcode.com/uploads/2021/02/19/rev2ex2.jpg)

```markdown
Input: head = [1,2,3,4,5], left = 2, right = 4
Output: [1,4,3,2,5]
```

### Solution

```java
public ListNode reverseBetween(ListNode head, int left, int right) {
    if(head == null || head.next == null)
        return head;

    ListNode sentinel = new ListNode(0, head);
    ListNode prev = sentinel, end = head;
    while(right > 1) {
        if(left > 1) {
            prev = prev.next;
            left--;
        }
        end = end.next;
        right--;
    }
    ListNode reverse = prev.next;
    ListNode next = end.next;
    end.next = null;
    prev.next = reverseList(reverse);
    reverse.next = next;
    return sentinel.next;
}

private ListNode reverseList(ListNode head) {
    if(head == null || head.next == null) 
        return head;

    ListNode cur = head.next, prev = head, next = null;
    while(cur != null) {
        next = cur.next;
        cur.next = prev;
        prev = cur;
        cur = next;
    }
    head.next = null;
    return prev;
}
```

***

## :star:Question 141

*Linked List Cycle*

### Description

Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

### Example

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

### Solution

* Quick slow pointer

* When both pointers are in the circle, assume the relative distance (the distance that quick pointer need to catch up) is D, then if the difference between two pointer's speeds is one node, then D will absolutely decrease to 0 

* If the difference is not one node, then they may not meet:

  Assume circle has N nodes, quick speed is Q, slow speed is S (Q > S), after k iteration two pointers meet (k is integer), then `k(Q - S) - D` must be divisible by `N`, which is not always true

  E.g., 4 nodes, slow at index 0, quick at index 3, slow's speed is one node, quick's speed is three nodes, they will never meet

```java
public boolean hasCycle(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;

    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;

        if (fast == slow)
            return true;
    }
    return false;
}
```

***

## :star:Question 142

*Linked List Cycle II*

### Description

Given the `head` of a linked list, return *the node where the cycle begins. If there is no cycle, return* `null`.

**Do not modify** the linked list.

### Example

```markdown
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
```

### Solution

* Upgrade version of *Question 141*
* ![142](https://raw.githubusercontent.com/hadjShell/Leetcode-Solution-Java/main/imgs/142.png)
* Quick pointer move `xq`: `x1 + x2 + x3 + x2`, slow pointer move `xs`: `x1 + x2`, and `xq = 2xs`, so `x1 = x3`
* So, after quick and slow meet, let quick back to the head of the list and make it move one node each iteration as slow point does. When they meet again, they will be at the start node of the cycle

```java
public ListNode detectCycle(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;

    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;

        // there is a cycle
        if (fast == slow) {
            fast = head;
            while(fast != slow) {
                fast = fast.next;
                slow = slow.next;
            }
            return fast;
        }
    }

    return null;
}
```

***

## Question 143

*Reorder List*

### Description

You are given the head of a singly linked-list. The list can be represented as:

```
L0 → L1 → … → Ln - 1 → Ln
```

*Reorder the list to be on the following form:*

```
L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
```

You may not modify the values in the list's nodes. Only nodes themselves may be changed.

### Example

![img](https://assets.leetcode.com/uploads/2021/03/04/reorder1linked-list.jpg)

![img](https://assets.leetcode.com/uploads/2021/03/09/reorder2-linked-list.jpg)

### Solution

* Notice the algorithm used to find middle node in the list: **quick slow pointers with different step lengths**

```java
public void reorderList(ListNode head) {
        if(head == null || head.next == null)
            return;
        
        ListNode middle = head, tail = head;
        // find middle node
        while(tail.next != null && tail.next.next != null) {
            middle = middle.next;
            tail = tail.next.next;
        }
        
        tail = reverseList(middle.next);
        middle.next = null;
        
        ListNode cur = head, tmp = null;
        while(tail != null) {
            tmp = tail.next;
            tail.next = cur.next;
            cur.next = tail;
            cur = cur.next.next;
            tail = tmp;
        }
        
        return;
    }
    
    private ListNode reverseList(ListNode head) {
        if(head == null || head.next == null) 
            return head;

        ListNode cur = head.next, prev = head, next = null;
        while(cur != null) {
            next = cur.next;
            cur.next = prev;
            prev = cur;
            cur = next;
        }
        head.next = null;
        return prev;
    }
```

***

## :star:Question 160

### Description

*Intersection of Two Linked Lists*

### Example

Given the heads of two singly linked-lists `headA` and `headB`, return *the node at which the two lists intersect*. If the two linked lists have no intersection at all, return `null`.

**Note** that the linked lists must **retain their original structure** after the function returns.

![img](https://assets.leetcode.com/uploads/2021/03/05/160_example_1_1.png)

```markdown
Input: intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
Output: Intersected at '8'
```

### Solution

* **Key**: commutative law of addition, `a + b = b + a`
* <img src="https://raw.githubusercontent.com/hadjShell/Leetcode-Solution-Java/main/imgs/160.png" alt="160" style="zoom: 67%;" />

```java
// Solution 1
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    if(headA == null || headB == null)
        return null;

    ListNode currA = headA;
    ListNode currB = headB;
    int iteration = 0;

    while(currA != currB) {
        if(iteration == 4)
            return null;
        if(currA.next == null) {
            currA = headB;
            iteration++;
        }
        else
            currA = currA.next;

        if(currB.next == null) {
            currB = headA;
            iteration++;
        }
        else
            currB = currB.next;
    }
    return currA;
}

// Solution 2
// Longer list starts from part of it which has the same length of shorter one
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    int len1 = 0;
    int len2 = 0;
    ListNode h1 = headA;
    ListNode h2 = headB;
    while(h1 != null) {
        h1 = h1.next;
        len1++;
    }
    while(h2 != null) {
        h2 = h2.next;
        len2++;
    }

    while(len1 != len2) {
        if (len1 > len2) {
            headA = headA.next;
            len1--;
        } else {
            headB = headB.next;
            len2--;
        }
    }

    while (headA != headB) {
        headA = headA.next;
        headB = headB.next;
    }
    return headA;
}
```

***

## Question 202

*Happy Number*

### Description

Write an algorithm to determine if a number `n` is happy.

A **happy number** is a number defined by the following process:

- Starting with any positive integer, replace the number by the sum of the squares of its digits.
- Repeat the process until the number equals 1 (where it will stay), or it **loops endlessly in a cycle** which does not include 1.
- Those numbers for which this process **ends in 1** are happy.

Return `true` *if* `n` *is a happy number, and* `false` *if not*.

- `1 <= n <= 2^31 - 1`

### Example

```markdown
Input: n = 19
Output: true
Explanation:
1^2 + 9^2 = 82
8^2 + 2^2 = 68
6^2 + 8^2 = 100
1^2 + 0^2 + 0^2 = 1
```

### Solution

* Floyd's cycle-finding algorithm: two endings: loop in `1` or loop in a cycle without `1` (cycle may not start at the input value)

```java
// Solution 1: implicit LinkedList cycle problem
public boolean isHappy(int n) {
    int slow = squares(n);
    int fast = squares(squares(n));

    while(fast != slow) {
        slow = squares(slow);
        fast = squares(squares(fast));
    }

    return fast == 1;
}

private int squares(int n) {
    int ret = 0;
    while(n != 0) {
        int tmp = n % 10;
        ret += tmp * tmp;
        n /= 10;
    }
    return ret;
}

// Solution 2: hardcoding
public boolean isHappy(int n) {
    if((n==1)||(n==7))
        return true;
    else if(n<10)
        return false;
    int m=0;
    while(n!=0) {
        int tail=n%10;
        m+=tail*tail;
        n/=10;
    }
    return isHappy(m);
}
```

***

## Question 203

*Remove Linked List Elements*

### Description

Given the `head` of a linked list and an integer `val`, remove all the nodes of the linked list that has `Node.val == val`, and return *the new head*.

### Example

```markdown
Input: head = [1,2,6,3,4,5,6], val = 6
Output: [1,2,3,4,5]
```

### Solution

```java
public ListNode removeElements(ListNode head, int val) {
    if(head == null)
        return null;

    ListNode sentinel = new ListNode(0, head);
    ListNode cur = head, prev = sentinel;
    while(cur != null) {
        if(cur.val == val)
            prev.next = cur.next;
        else
            prev = cur;
        cur = cur.next;
    }

    return sentinel.next;
}
```

***

## Question 206

*Reverse Linked List*

### Description

Given the `head` of a singly linked list, reverse the list, and return *the reversed list*.

### Example

```markdown
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```

### Solution

```java
public ListNode reverseList(ListNode head) {
    if(head == null || head.next == null) 
        return head;

    ListNode cur = head.next, prev = head, next = null;
    while(cur != null) {
        next = cur.next;
        cur.next = prev;
        prev = cur;
        cur = next;
    }
    head.next = null;
    return prev;
}
```

***

## Question 234

*Palindrome Linked List*

### Description

Given the `head` of a singly linked list, return `true` if it is a palindrome.

### Example

```markdown
Input: head = [1,2,2,1]
Output: true
Input: head = [1,2]
Output: false
```

### Solution

* Same logic as [question 143](#question-143)
* Or can use array to store list information and then check, but the memory complexity will be `O(N)`

```java
public boolean isPalindrome(ListNode head) {
    if(head == null || head.next == null)
        return true;

    ListNode middle = head, tail = head;
    // find middle node
    while(tail.next != null && tail.next.next != null) {
        middle = middle.next;
        tail = tail.next.next;
    }
    tail = reverseList(middle.next);

    while(tail != null) {
        if(head.val != tail.val)
            return false;
        tail = tail.next;
        head = head.next;
    }

    return true;
}

private ListNode reverseList(ListNode head) {
    if(head == null || head.next == null) 
        return head;

    ListNode cur = head.next, prev = head, next = null;
    while(cur != null) {
        next = cur.next;
        cur.next = prev;
        prev = cur;
        cur = next;
    }
    head.next = null;
    return prev;
}
```

***

## Question 237

*Delete Node in a Linked List*

### Description

Write a function to **delete a node** in a singly-linked list. You will **not** be given access to the `head` of the list, instead you will be given access to **the node to be deleted** directly.

It is **guaranteed** that the node to be deleted is **not a tail node** in the list.

- The number of the nodes in the given list is in the range `[2, 1000]`.

### Example

```markdown
Input: head = [4,5,1,9], node = 5
Output: [4,1,9]
```

### Solution

```java
public void deleteNode(ListNode node) {
        node.val = node.next.val;
        node.next = node.next.next;
    }
```

***

## Question 258

*Add Digits*

### Description

Given an integer `num`, repeatedly add all its digits until the result has only one digit, and return it.

### Example

```markdown
Input: num = 38
Output: 2
Explanation: The process is
38 --> 3 + 8 --> 11
11 --> 1 + 1 --> 2 
Since 2 has only one digit, return it.
```

### Solution

```java
// Solution 1: recursion
public int addDigits(int num) {
    if(num < 10)
        return num;
    else {
        int newNum = 0;
        while(num != 0) {
            newNum += num % 10;
            num /= 10;
        }
        return addDigits(newNum);
    }
}

// Solution 2: math
public int addDigits(int num) {
    return 1 + (num - 1) % 9;
}
```

***

## Question

### Description

### Example

### Solution
