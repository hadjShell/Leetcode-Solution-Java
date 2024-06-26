# Leetcode Solution

## Binary Search

* **Locate the section on which you want to search based on the situation**
* Change the **base case or recursive case logic** of a normal binary search
* Be aware of the out of bound problem

### Q34. [Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

* ```java
  class Solution {
      public int[] searchRange(int[] nums, int target) {
          int len = nums.length;
          if (len == 0)   return new int[] {-1, -1};
  
          int first = bsFirst(nums, target, 0, len - 1);
          if (first == -1) {
              return new int[] {-1, -1};
          }
          else {
              return new int[] {first, bsLast(nums, target, 0, len - 1)};
          }
      }
  
      private int bsFirst(int[] nums, int target, int left, int right) {
          if (left > right)   return -1;
          int mid = left + (right - left) / 2;
          int tmp = mid;
          if (target > nums[mid])
              return bsFirst(nums, target, mid + 1, right);
          else if (target < nums[mid])
              return bsFirst(nums, target, left, mid - 1);
          else {
              if (mid == left || nums[mid - 1] != target)
                  return mid;
              else
                  return bsFirst(nums, target, left, mid - 1);
          }
      }
  
      private int bsLast(int[] nums, int target, int left, int right) {
          if (left > right)   return -1;
          int mid = left + (right - left) / 2;
          int tmp = mid;
          if (target > nums[mid])
              return bsLast(nums, target, mid + 1, right);
          else if (target < nums[mid])
              return bsLast(nums, target, left, mid - 1);
          else {
              if (mid == right || nums[mid + 1] != target)
                  return mid;
              else
                  return bsLast(nums, target, mid + 1, right);
          }
      }
  }
  ```

### Q35. [Search Insert Position](https://leetcode.com/problems/search-insert-position/)

* ```java
  class Solution {
      public int searchInsert(int[] nums, int target) {
          if(nums.length == 0)    return 0;
          return bs(nums, target, 0, nums.length - 1);
      }
  
      private int bs(int[] nums, int target, int left, int right) {
          if (left > right) return left;
          int mid = left + (right - left) / 2;
          if (nums[mid] > target)
              return bs(nums, target, left, mid - 1);
          else if (nums[mid] < target)
              return bs(nums, target, mid + 1, right);
          else
              return mid;
      }
  }
  ```

### Q74. [Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)

* ```java
  class Solution {
      public boolean searchMatrix(int[][] matrix, int target) {
          return bsMatrix(matrix, target, 0, matrix.length - 1);
      }
  
      private boolean bsMatrix(int[][] matrix, int target, int left, int right) {
          if (left > right)   return false;
          int mid = left + (right - left) / 2;
          if (bs(matrix[mid], target, 0, matrix[0].length - 1) == true)
              return true;
          else {
              if (target < matrix[mid][0])
                  return bsMatrix(matrix, target, left, mid - 1);
              else
                  return bsMatrix(matrix, target, mid + 1, right);
          }
      }
  
      private boolean bs(int[] nums, int target, int left, int right) {
          if (left > right)   return false;
          int mid = left + (right - left) / 2;
          if (target > nums[mid])
              return bs(nums, target, mid + 1, right);
          else if (target < nums[mid])
              return bs(nums, target, left, mid - 1);
          else 
              return true;
      }
  }
  ```

### Q33. [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

* It's about to consider all possibilities

* ```java
  class Solution {
      public int search(int[] nums, int target) {
          return bs(nums, target, 0, nums.length - 1);
      }
  
      private int bs(int[] nums, int target, int left, int right) {
          if (left > right)   return -1;
  
          int mid = left + (right - left) / 2;
          if (target == nums[mid])    return mid;
  
          // mid in large section
          if (nums[mid] > nums[right]) {
              if (target < nums[mid] && target >= nums[left])
                  return bs(nums, target, left, mid - 1);
              else
                  return bs(nums, target, mid + 1, right);
          }
          // mid in small section
          else if (nums[mid] < nums[left])
              if (target > nums[mid] && target <= nums[right])
                  return bs(nums, target, mid + 1, right);
              else
                  return bs(nums, target, left, mid - 1);
          // no rotate
          else {
              if (target > nums[mid])
                  return bs(nums, target, mid + 1, right);
              else
                  return bs(nums, target, left, mid - 1);
          }
      }
  }
  ```

### Q81. [Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)

* Q33 + Q154

* ```java
  class Solution {
      public boolean search(int[] nums, int target) {
          return bsDuplicate(nums, target, 0, nums.length - 1);
      }
  
      private boolean bsDuplicate(int[] nums, int target, int left, int right) {
          if (left == right)  return nums[left] == target;
          else if (left + 1 == right)
              return nums[left] == target || nums[right] == target;
          
          // at least 3 elements 
          int mid = left + (right - left) / 2;
          if (nums[mid] == target)    return true;
          if (nums[left] > nums[right]) {
              if (nums[mid] > nums[right]) {
                  if (target < nums[mid] && target >= nums[left])
                      return bsDuplicate(nums, target, left, mid - 1);
                  else 
                      return bsDuplicate(nums, target, mid + 1, right);
              }
              else {
                  if (target > nums[mid] && target <= nums[right])
                      return bsDuplicate(nums, target, mid + 1, right);
                  else
                      return bsDuplicate(nums, target, left, mid - 1);
              }
          }
          else if (nums[left] < nums[right]) {
              if (target < nums[mid])
                  return bsDuplicate(nums, target, left, mid - 1);
              else
                  return bsDuplicate(nums, target, mid + 1, right);
          }
          else 
              return bsDuplicate(nums, target, left, mid - 1) || bsDuplicate(nums, target, mid + 1, right);
      }
  }
  ```

### Q153. [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

* ```java
  class Solution {
      public int findMin(int[] nums) {
          return bsMin(nums, 0, nums.length - 1);
      }
  
      private int bsMin(int[] nums, int left, int right) {
          if (left == right)  return nums[left];
          int mid = left + (right - left) / 2;
          // mid in large part
          if (nums[mid] >= nums[left] && nums[mid] > nums[right]) 
              return bsMin(nums, mid + 1, right);
          // mid in small part
          else if (nums[mid] < nums[left]) {
              if (nums[mid] < nums[mid - 1])
                  return nums[mid];
              else
                  return bsMin(nums, left, mid - 1);
          }
          // no rotate
          else
              return nums[left];
      }
  }
  ```

### Q154. [Find Minimum in Rotated Sorted Array II](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/)

* Two solutions: recursion or loop

* ```java
  class Solution {
      public int findMin(int[] nums) {
          return bsMinDuplicate(nums, 0, nums.length - 1);
      }
  
      private int bsMinDuplicate(int[] nums, int left, int right) {
          if (left == right)  return nums[left];
          else if (left + 1 == right)
              return nums[left] < nums[right] ? nums[left] : nums[right];
          
          // at least 3 elements 
          int mid = left + (right - left) / 2;
          if (nums[left] > nums[right]) {
              if (nums[mid] > nums[right])
                  return bsMinDuplicate(nums, mid + 1, right);
              else
                  return nums[mid] < nums[mid - 1] ? nums[mid] : bsMinDuplicate(nums, left, mid - 1);
          }
          else if (nums[left] < nums[right])
              return nums[left];
          else {
              if (nums[mid] < nums[mid - 1] && nums[mid] < nums[mid + 1])
                  return nums[mid];
              else {
                  int leftMin = bsMinDuplicate(nums, left, mid - 1);
                  int rightMin = bsMinDuplicate(nums, mid + 1, right);
                  return leftMin < rightMin ? leftMin : rightMin;
              }
          }
      }
  }
  ```

* ```java
  class Solution {
      public int findMin(int[] nums) {
           int l = 0;
          int h = nums.length - 1;
          
          while (l < h) {
              int m = l + (h - l) / 2;
  
              // Handle duplicates
              if (nums[l] == nums[m] && nums[m] == nums[h]) {
                  l++;
                  h--;
              } else if (nums[m] <= nums[h]) {
                  // Right part is sorted or pivot element is to the left
                  h = m;
              } else {
                  // Left part is sorted and pivot element is to the right
                  l = m + 1;
              }
          }
          
          return nums[l];
  
      }
  }
  ```

### Q278. [First Bad Version](https://leetcode.com/problems/first-bad-version/)

* ```java
  /* The isBadVersion API is defined in the parent class VersionControl.
        boolean isBadVersion(int version); */
  
  public class Solution extends VersionControl {
      public int firstBadVersion(int n) {
          return bs(0, n - 1);
      }
  
      private int bs(int left, int right) {
          if (left > right)   return -1;
          int mid = left + (right - left) / 2;
          if (isBadVersion(mid + 1)) {
              if (isBadVersion(mid))
                  return bs(left, mid - 1);
              else
                  return mid + 1;
          }
          else
              return bs(mid + 1, right);
      }
  }
  ```

***

## Linked List

* Corner case
  * Empty list
  * Single node
  * Two nodes
  * Operation on head node (and tail node for doubly linked list)

### Q19. [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

* ```java
  class Solution {
      public ListNode removeNthFromEnd(ListNode head, int n) {
          ListNode cur = head;
          int len = 1;
          while (cur.next != null) {
              cur = cur.next;
              len++;
          }
  
          if (len == n)
              return head.next;
          
          cur = head;
          for (int i = 1; i < len - n; i++) {
              cur = cur.next;
          }
          cur.next = cur.next.next;
          return head;
      }
  }
  ```



***

## Two Pointers

* Fast and slow pointers
* Left and right pointers

### Q21. [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

* Sentinel node + two pointers

* ```java
  class Solution {
      public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
          if (list1 == null)  return list2;
          if (list2 == null)  return list1;
  
          ListNode head = new ListNode();
          ListNode n = head;
          while (list1 != null && list2 != null) {
              if (list2.val <= list1.val) {
                  n.next = list2;
                  list2 = list2.next;
              }
              else {
                  n.next = list1;
                  list1 = list1.next;
              }
              n = n.next;
          }
          if (list1 == null)
              n.next = list2;
          else 
              n.next = list1;
          
          return head.next;
      }
  }
  ```

* ```java
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

### Q23. [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

* Divide and conquer + Q21.

* ```java
  class Solution {
      public ListNode mergeKLists(ListNode[] lists) {
          return merge(lists, 0, lists.length - 1);
      }
  
      public ListNode merge(ListNode[] lists, int left, int right) {
          if (left > right)
              return null;
          else if (left == right) 
              return lists[left];
          else {
              int mid = left + (right - left) / 2;
              ListNode leftList = merge(lists, left, mid), rightList = merge(lists, mid + 1, right);
              return mergeTwoLists(leftList, rightList);
          }  
      }
  
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

### Q86. [Partition List](https://leetcode.com/problems/partition-list/)

* Sentinel node

* ```java
  class Solution {
      public ListNode partition(ListNode head, int x) {
          if (head == null || head.next == null)
              return head;
          
          ListNode less = new ListNode(0);
          ListNode great = new ListNode(0);
          ListNode l = less, g = great;
  
          while (head != null) {
              if (head.val < x) {
                  less.next = head;
                  less = less.next;
              }
              else {
                  great.next = head;
                  great = great.next;
              }
              head = head.next;
          }
        	// the last node may point to uncorrect node
          great.next = null;
          less.next = g.next;
          return l.next;
      }
  }
  ```



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

## Question 13

*Roman to Integer*

### Description

Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

```markdown
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

For example, `2` is written as `II` in Roman numeral, just two ones added together. `12` is written as `XII`, which is simply `X + II`. The number `27` is written as `XXVII`, which is `XX + V + II`.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:

- `I` can be placed before `V` (5) and `X` (10) to make 4 and 9. 
- `X` can be placed before `L` (50) and `C` (100) to make 40 and 90. 
- `C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.

Given a roman numeral, convert it to an integer.

- `1 <= s.length <= 15`
- It is **guaranteed** that `s` is a valid roman numeral in the range `[1, 3999]`.

### Example

```markdown
Input: s = "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

### Solution

```java
public int romanToInt(String s) {
    Map<Character, Integer> m = new HashMap<>();
    m.put('I', 1);
    m.put('V', 5);
    m.put('X', 10);
    m.put('L', 50);
    m.put('C', 100);
    m.put('D', 500);
    m.put('M', 1000);

    int ret = 0;
    char[] letters = s.toCharArray();
    int prev = 0;
    for(int i = letters.length - 1; i > -1; i--) {
        int val = m.get(letters[i]);
        if(val >= prev)
            ret += val;
        else
            ret -= val;
        prev = val;
    }
    return ret;
}
```

***

## Question 14

*Longest Common Prefix*

### Description

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

- `1 <= strs.length <= 200`
- `0 <= strs[i].length <= 200`
- `strs[i]` consists of only lowercase English letters.

### Example

```markdown
Input: strs = ["flower","flow","flight"]
Output: "fl"
```

### Solution

```java
public String longestCommonPrefix(String[] strs) {
    String ret = strs[0];
    for(int i = 1; i < strs.length; i++) {
        int j = 0, k = 0;
        for(; j < ret.length() && k < strs[i].length(); j++, k++) {
            if(ret.charAt(j) != strs[i].charAt(k)) {
                ret = ret.substring(0, j);
                break;
            }
        }   
        // ret is not substringed and is longer than strs[i]
        if(j != ret.length())
            ret = ret.substring(0, j);
    }
    return ret;
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

## Question 26

*Remove Duplicates from Sorted Array*

### Description

Given an integer array `nums` sorted in **non-decreasing order**, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each unique element appears only **once**. The **relative order** of the elements should be kept the **same**.

Return `k` *after placing the final result in the first* `k` *slots of* `nums`.

### Example

```markdown
Input: nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums being 0, 1, 2, 3, and 4 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

### Solution

* Two pointer
* Create a new array using the old array (there is actually one array)

```java
public int removeDuplicates(int[] nums) {
    int cur = 1, ret = 1;
    while(cur < nums.length) {
        if(nums[cur] != nums[cur - 1]) {
            nums[ret++] = nums[cur];
        }
        cur++;
    }
    return ret;
}
```

***

## Question 27

*Remove Element*

### Description

Given an integer array `nums` and an integer `val`, remove all occurrences of `val` in `nums` [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm). The relative order of the elements may be changed.

Return `k` *after placing the final result in the first* `k` *slots of* `nums`.

### Example

```markdown
Input: nums = [0,1,2,2,3,0,4,2], val = 2
Output: 5, nums = [0,1,4,0,3,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums containing 0, 0, 1, 3, and 4.
Note that the five elements can be returned in any order.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

### Solution

* Two pointers

```java
public int removeElement(int[] nums, int val) {
    int i = 0, j = nums.length - 1;
    while(i <= j) {
        if(nums[i] != val)
            i++;
        else {
            nums[i] = nums[j];
            nums[j] = val;
            j--;
        }
    }
    return j + 1;
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

## Question 66

*Plus One*

### Description

You are given a **large integer** represented as an integer array `digits`, where each `digits[i]` is the `ith` digit of the integer. The digits are ordered from most significant to least significant in left-to-right order. The large integer does not contain any leading `0`'s.

Increment the large integer by one and return *the resulting array of digits*.

### Example

```markdown
Input: digits = [1,2,3]
Output: [1,2,4]
Input: digits = [9]
Output: [1,0]
```

### Solution

```java
public int[] plusOne(int[] digits) {
    int carry = 1;
    for(int i = digits.length - 1; i > - 1; i--) {
        if(carry == 1) {
            digits[i] = (digits[i] + carry) % 10;
            carry = digits[i] == 0 ? 1 : 0;
        }
        if(carry == 0)
            break;
    }
    if(carry == 1) {
        digits = new int[digits.length + 1];
        digits[0] = 1;
    }
    return digits;
}
```

***

## Question 75

*Sort Colors*

### Description

Given an array `nums` with `n` objects colored red, white, or blue, sort them **[in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers `0`, `1`, and `2` to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.

### Example

```markdown
Input: nums = [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```

### Solution

```java
public void sortColors(int[] nums) {
    int i = 0, j = 0, k = 0;
    for(int num : nums) {
        switch(num) {
            case 0:
                i++;
                break;
            case 1:
                j++;
                break;
            case 2:
                k++;
                break;
            default:
                break;
        }
    }
    int cur = 0;
    for(int a = 0; a < i; a++) {
        nums[cur++] = 0;
    }
    for(int a = 0; a < j; a++) {
        nums[cur++] = 1;
    }
    for(int a = 0; a < k; a++) {
        nums[cur++] = 2;
    }
}
```

***

## Question 80

*Remove Duplicates from Sorted Array II*

### Description

Given an integer array `nums` sorted in **non-decreasing order**, remove some duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each unique element appears **at most twice**. The **relative order** of the elements should be kept the **same**.

### Example

```markdown
Input: nums = [1,1,1,2,2,3]
Output: 5, nums = [1,1,2,2,3,_]
Explanation: Your function should return k = 5, with the first five elements of nums being 1, 1, 2, 2 and 3 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

### Solution

* Upgraded version of [question 26](#question-26)

```java
public int removeDuplicates(int[] nums) {
    int cur = 1, ret = 1, appear = 1;
    while(cur < nums.length) {
        if(nums[cur] == nums[cur - 1]) {
            if(appear == 1)
                nums[ret++] = nums[cur];
            appear++;   
        }
        else {
            nums[ret++] = nums[cur];
            appear = 1;
        }
        cur++;
    }
    return ret;
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

## Question 88

*Merge Sorted Array*

### Description

You are given two integer arrays `nums1` and `nums2`, sorted in **non-decreasing order**, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.

**Merge** `nums1` and `nums2` into a single array sorted in **non-decreasing order**.

The final sorted array should not be returned by the function, but instead be *stored inside the array* `nums1`. To accommodate this, `nums1` has a length of `m + n`, where the first `m` elements denote the elements that should be merged, and the last `n` elements are set to `0` and should be ignored. `nums2` has a length of `n`.

### Example

```markdown
Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.
```

### Solution

* Two pointers

```java
public void merge(int[] nums1, int m, int[] nums2, int n) {
    int i = m - 1, j = n - 1;
    while(i > -1 && j > -1) {
        nums1[i + j + 1] = (nums2[j] >= nums1[i]) ? nums2[j--] : nums1[i--];
    }
    if(i == -1) {
        while(j > -1) {
            nums1[j] = nums2[j];
            j--;
        }
    }
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

## Question 125

*Valid Palindrome*

### Description

A phrase is a **palindrome** if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string `s`, return `true` *if it is a **palindrome**, or* `false` *otherwise*.

### Example

```markdown
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.
```

### Solution

```java
public boolean isPalindrome(String s) {
    char[] letters = new char[s.length()];
    int len = 0;
    for(char c : s.toCharArray()) {
        if(Character.isLetterOrDigit(c))
            letters[len++] = Character.toLowerCase(c);
    }
    for(int i = 0; i < len / 2; i++) {
        if(letters[i] != letters[len - 1 - i])
            return false;
    }
    return true;
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

## Question 225

*Implement Stack using Queues*

### Description

Implement a last-in-first-out (LIFO) stack using only two queues. The implemented stack should support all the functions of a normal stack (`push`, `top`, `pop`, and `empty`).

Implement the `MyStack` class:

- `void push(int x)` Pushes element x to the top of the stack.
- `int pop()` Removes the element on the top of the stack and returns it.
- `int top()` Returns the element on the top of the stack.
- `boolean empty()` Returns `true` if the stack is empty, `false` otherwise.

**Notes:**

- You must use **only** standard operations of a queue, which means that only `push to back`, `peek/pop from front`, `size` and `is empty` operations are valid.
- Depending on your language, the queue may not be supported natively. You may simulate a queue using a list or deque (double-ended queue) as long as you use only a queue's standard operations.

### Example

### Solution

* Reverse queue using `dequeue()` and `enqueue()`

```java
class MyStack {
    Queue<Integer> q;
    public MyStack() {
		q = new LinkedList<>();
    }

    public void push(int x) {
        q.add(x);
        for(int i = 1; i < q.size(); i++) {
            q.add(q.remove());
        }
    }

    public int pop() {
        return q.remove();
    }

    public int top() {
        return q.peek();
    }

    public boolean empty() {
        return q.isEmpty();
    }
}
```

***

## :star:Question 232

*Implement Queue using Stacks*

### Description

Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (`push`, `peek`, `pop`, and `empty`).

Implement the `MyQueue` class:

- `void push(int x)` Pushes element x to the back of the queue.
- `int pop()` Removes the element from the front of the queue and returns it.
- `int peek()` Returns the element at the front of the queue.
- `boolean empty()` Returns `true` if the queue is empty, `false` otherwise.

**Notes:**

- You must use **only** standard operations of a stack, which means only `push to top`, `peek/pop from top`, `size`, and `is empty` operations are valid.
- Depending on your language, the stack may not be supported natively. You may simulate a stack using a list or deque (double-ended queue) as long as you use only a stack's standard operations.

### Example

### Solution

* [Similar question 225](#question-225), but totally different logic
* Use two stacks to get and reverse the list
* The first time `pop()` is invoked, `stackOut`(queue) is created and `stackIn`(stack) is clear. There is no need to reverse `stackOut` back to `stackIn` since `MyQueue` is FIFO, so that **each operation is [amortized](https://en.wikipedia.org/wiki/Amortized_analysis) `O(1)` time complexity.**

```java
// Solution 1
// reverse from s1 to s2, operation, reverse back
class MyQueue {
    private Stack<Integer> s1;
    private Stack<Integer> s2;

    public MyQueue() {
        s1 = new Stack<>();
        s2 = new Stack<>();
    }
    
    public void push(int x) {
        s1.push(x);
    }
    
    public int pop() {
        while(!s1.isEmpty()) {
            s2.push(s1.pop());
        }
        int value = s2.pop();
        while(!s2.isEmpty()) {
            s1.push(s2.pop());
        }
        return value;
    }
    
    public int peek() {
        while(!s1.isEmpty()) {
            s2.push(s1.pop());
        }
        int value = s2.peek();
        while(!s2.isEmpty()) {
            s1.push(s2.pop());
        }
        return value;
    }
    
    public boolean empty() {
        return s1.isEmpty();
    }
}

// Upgraded version
// More efficient
class MyQueue {
    Stack<Integer> stackIn;
    Stack<Integer> stackOut;

    public MyQueue() {
        stackIn = new Stack<>();
        stackOut = new Stack<>();  
    }

    public void push(int x) {
        stackIn.push(x); 
    }

    public int pop() {
        if(stackOut.isEmpty()) {
            while(!stackIn.isEmpty()) {
                stackOut.push(stackIn.pop());
            }
        }
        return stackOut.pop();    
    }

    public int peek() {
        // in case stackOut is not created
        int res = this.pop();
        stackOut.push(res);
        return res;
    }

    public boolean empty() {
        return stackIn.isEmpty() && stackOut.isEmpty();    
    }
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

## Question 242

*Valid Anagram*

### Description

Given two strings `s` and `t`, return `true` *if* `t` *is an anagram of* `s`*, and* `false` *otherwise*.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

### Example

```markdown
Input: s = "anagram", t = "nagaram"
Output: true
Input: s = "ab", t = "a"
Output: false
```

### Solution

```java
public boolean isAnagram(String s, String t) {
    if(s.length() != t.length())
        return false;
    int[] a = new int[26];
    for(char c : s.toCharArray()) {
        a[c - 'a']++;
    }
    for(char c : t.toCharArray()) {
        a[c - 'a']--;
    }
    for(int i = 0; i < 26; i++) {
        if(a[i] != 0)
            return false;
    }
    return true;
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

## Question 344

*Reverse String*

### Description

Write a function that reverses a string. The input string is given as an array of characters `s`.

You must do this by modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm) with `O(1)` extra memory

### Example

```markdown
Input: s = ["h","e","l","l","o"]
Output: ["o","l","l","e","h"]
```

### Solution

```java
public void reverseString(char[] s) {
    for(int i = 0; i < s.length / 2; i++) {
        char tmp = s[i];
        s[i] = s[s.length - 1 - i];
        s[s.length - 1 - i] = tmp;
    }
}
```

***

## :star:Question 890

*Find and Replace Pattern*

### Description

Given a list of strings `words` and a string `pattern`, return *a list of* `words[i]` *that match* `pattern`. You may return the answer in **any order**.

A word matches the pattern if there exists a permutation of letters `p` so that after replacing every letter `x` in the pattern with `p(x)`, we get the desired word.

Recall that a permutation of letters is a bijection from letters to letters: every letter maps to another letter, and no two letters map to the same letter.

- `1 <= pattern.length <= 20`
- `1 <= words.length <= 50`
- `words[i].length == pattern.length`
- `pattern` and `words[i]` are lowercase English letters.

### Example

```markdown
Input: words = ["abc","deq","mee","aqq","dkd","ccc"], pattern = "abb"
Output: ["mee","aqq"]
Explanation: "mee" matches the pattern because there is a permutation {a -> m, b -> e, ...}. 
"ccc" does not match the pattern because {a -> c, b -> c, ...} is not a permutation, since a and b map to the same letter.
```

### Solution

* For solution 2, `charArray` implicitly created a HashMap mapping distinct character to first present index

```java
// Solution 1: HashMap
public List<String> findAndReplacePattern(String[] words, String pattern) {
    List<String> ret = new ArrayList<>();
    Map<Character, Character> m = new HashMap<>();
    char[] p = pattern.toCharArray();
    for(String word : words) {
        char[] w = word.toCharArray();
        m.clear();
        int i = 0;
        for(; i < p.length; i++) {
            if(!m.containsKey(p[i])) {
                if(m.containsValue(w[i]))
                    break;
                else {
                    m.put(p[i], w[i]);
                }
            }
            else {
                if(m.get(p[i]) != w[i])
                    break;
            }
        }
        if(i == p.length)   ret.add(word);
    }
    return ret;
}

// Solution 2
public static List<String> findAndReplacePattern(String[] words, String pattern) {
    List<String> list=new LinkedList<>();

    for(int i=0;i<words.length;i++){
        if(checkPattern(words[i],pattern)){
            list.add(words[i]);
        }
    }
    return list;
}

private static boolean checkPattern(String word, String pattern){
    for(int i=0;i<word.length();i++){
        if(word.indexOf(word.charAt(i))!=pattern.indexOf(pattern.charAt(i)))
            return false;
    }
    return true;
}
```

***

## Question

### Description

### Example

### Solution
