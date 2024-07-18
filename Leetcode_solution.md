# Leetcode Solution

## Array

### :star:Q605. [Can Place Flowers](https://leetcode.com/problems/can-place-flowers/)

* ```java
  class Solution {
      public boolean canPlaceFlowers(int[] flowerbed, int n) {
          int pointer = 0;
          while(pointer < flowerbed.length && n>=0){
              if(flowerbed[pointer] == 1){
                  pointer += 2;
              } else{
                  if(pointer + 1 > flowerbed.length - 1){
                      n--;
                      pointer += 1;
                  } else if(flowerbed[pointer + 1] == 0){
                      n--;
                      pointer += 2;
                  } else{
                      pointer += 3; 
                  }
              }
          }
          return n <= 0;
      }
  }
  ```

### Q1431. [Kids With the Greatest Number of Candies](https://leetcode.com/problems/kids-with-the-greatest-number-of-candies/)

* ```java
  class Solution {
      public List<Boolean> kidsWithCandies(int[] candies, int extraCandies) {
          int max = 0;
          for (int c : candies) {
              if (max < c)
                  max = c;
          }
          List<Boolean> l = new ArrayList<>();
          for (int c : candies) {
              if (c + extraCandies >= max)
                  l.add(true);
              else
                  l.add(false);
          }
          return l;
      }
  }
  ```

***

## Linked List

* Tricks
  * Two pointers
  * Dummy node

* Corner case
  * Empty list
  * Single node
  * Two nodes
  * Operation on head node (and tail node for doubly linked list)

***

## String

### Q224. [Basic Calculator](https://leetcode.com/problems/basic-calculator/)

* ```java
  
  ```

* ==Need to be done again==


### :star:Q1071. [Greatest Common Divisor of Strings](https://leetcode.com/problems/greatest-common-divisor-of-strings/)

* ```java
  class Solution {
      public String gcdOfStrings(String str1, String str2) {
          if(!(str1 + str2).equals(str2+str1))
              return "";
  
          int len1 = str1.length(),len2 = str2.length();
          return str1.substring(0,gcd(len1,len2));
      }
  
      // private String dOfString(String str) {
      //     StringBuilder div = new StringBuilder();
      //     char[] c = str.toCharArray();
      //     for (int i = 0; i < c.length; i++) {
      //         div.append(c[i]);
      //         StringBuilder tmp = new StringBuilder(div);
      //         StringBuilder s = new StringBuilder(str);
      //         if (tmp.append(str).compareTo(s.append(div)) == 0)
      //             break;
      //     }
      //     return div.toString();
      // }
  
      private int gcd(int m, int n) {
          int t = 1;
          while (t != 0) {
              t = m % n;
              m = n;
              n = t;
          }
          return m;
      }
  }
  ```

### Q1768. [Merge Strings Alternately](https://leetcode.com/problems/merge-strings-alternately/)

* ```java
  class Solution {
      public String mergeAlternately(String word1, String word2) {
          int len1 = word1.length(), len2 = word2.length();
          StringBuilder word = new StringBuilder();
          char[] w1 = word1.toCharArray(), w2 = word2.toCharArray();
          int i = 0, j = 0;
          for (; i < len1 && j < len2; i++, j++) {
              word.append(w1[i]).append(w2[j]);
          }
          if (len2 > len1)
              while (j < len2) 
                  word.append(w2[j++]);
          else 
              while (i < len1)
                  word.append(w1[i++]);
          return word.toString();
      }
  }
  ```

***

## Two Pointers

* Fast and slow pointers
  * Find middle element
  * Circular list

* Left and right pointers
  * Reverse array

* Sliding window
  * Subarray
  * Find Last k element

* Etc.

#### Fast and slow

### Q141. [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)

* ```java
  public class Solution {
      public boolean hasCycle(ListNode head) {
          ListNode fast = head, slow = head;
          while (fast != null && fast.next != null) {
              fast = fast.next.next;
              slow = slow.next;
              if (fast == slow)   return true;
          }
          return false;
      }
  }
  ```

### Q142. [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

* <img src="imgs/142.png" alt="142" style="zoom:50%;" />

* Quick pointer move `xq`: `x1 + x2 + x3 + x2`, slow pointer move `xs`: `x1 + x2`, and `xq = 2xs`, so `x1 = x3`

* So, after quick and slow meet, let quick back to the head of the list and make it move one node each iteration as slow point does. When they meet again, they will be at the start node of the cycle

* ```java
  public class Solution {
      public ListNode detectCycle(ListNode head) {
          ListNode fast = head, slow = head;
          while (fast != null && fast.next != null) {
              fast = fast.next.next;
              slow = slow.next;
              if (fast == slow) {
                  fast = head;
                  while (fast != slow) {
                      slow = slow.next;
                      fast = fast.next;
                  }
                  return fast;
              }
          }
          return null;
      }
  }
  ```

### Q876. [Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)

* ```java
  class Solution {
      public ListNode middleNode(ListNode head) {
          ListNode fast = head, slow = head;
          while (fast != null && fast.next != null) {
              fast = fast.next.next;
              slow = slow.next;
          }
          return slow;
      }
  }
  ```

### Q2095. [Delete the Middle Node of a Linked List](https://leetcode.com/problems/delete-the-middle-node-of-a-linked-list/)

* ```java
  class Solution {
      public ListNode deleteMiddle(ListNode head) {
          if (head.next == null)
              return null;
          
          ListNode s = head;
          ListNode f = head.next.next;
          while (f != null && f.next != null) {
              s = s.next;
              f = f.next.next;
          }
          s.next = s.next.next;
          return head;
      }
  }
  ```

### Q2130. [Maximum Twin Sum of a Linked List](https://leetcode.com/problems/maximum-twin-sum-of-a-linked-list/)

* ```java
  class Solution {
      public int pairSum(ListNode head) {
          ListNode f = head.next.next, s = head;
          ListNode reverse = new ListNode();
          while (f != null) {
              f = f.next.next;
              ListNode tmp = s;
              s = s.next;
              tmp.next = reverse;
              reverse = tmp;
          }
          f = s.next;
          s.next = reverse;
  
          int max = 0;
          while (f != null) {
              int sum = s.val + f.val;
              max = sum > max ? sum : max;
              s = s.next;
              f = f.next;
          }
  
          return max;
      }
  }
  ```

***

#### Left and right

### :star:Q11. [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

* Better: Eliminate the possiblities

* ```java
  class Solution {
      public int maxArea(int[] height) {
          int i = 0, j = height.length - 1, max = 0;
          while (i < j) {
              int v = (j - i) * Math.min(height[i], height[j]);
              if (max < v)    max = v;
              if (height[i] <= height[j]) i++;
              else                        j--;
          }
          return max;
      }
  }
  ```

* Best: The problem is to find a greater height for smaller width during we scan the array

* ```java
  class Solution {
      public int maxArea(int[] height) {
          int i = 0, j = height.length - 1, max = 0;
          while (i < j) {
              int h = Math.min(height[i], height[j]);
              int v = (j - i) * h;
              if (max < v)    max = v;
              while (i < j && height[i] <= h)     i++;
              while (i < j && height[j] <= h)     j--;
          }
          return max;
      }
  }
  ```

### Q345. [Reverse Vowels of a String](https://leetcode.com/problems/reverse-vowels-of-a-string/)

* ```java
  class Solution {
      public String reverseVowels(String s) {
          char[] c = s.toCharArray();
          int h = 0, t = c.length - 1;
          while (h < t) {
              while (h < c.length && !isVowel(c[h]))
                  h++;
              while (t >= 0 && !isVowel(c[t]))
                  t--;
              if (h < t) {
                  char tmp = c[h];
                  c[h] = c[t];
                  c[t] = tmp;
                  h++;
                  t--;
              }
          }
          return new String(c);
      }
  
      private boolean isVowel(char c) {
          return switch (c) {
              case 'a', 'e', 'i', 'o', 'u',
                   'A', 'E', 'I', 'O', 'U' -> true;
              default -> false;
          };
      }
  }
  ```

***

#### Sliding window

* When to increase the window
* When to shrink the window
* When to update the result

### :star:Q3. [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

* ```java
  class Solution {
      public int lengthOfLongestSubstring(String s) {
          int l = 0, r = l, maxLen = 0;
          Map<Character, Integer> cs = new HashMap<>();
          while (r < s.length()) {
              char c = s.charAt(r);
              if (cs.containsKey(c)) {
                  int p = cs.get(c);
                  l = p < l ? l : p + 1;
              }
              cs.put(c, r);
              maxLen = Math.max(maxLen, r - l + 1);
              r++;
          }
          return maxLen;
      }
  }
  ```

### Q19. [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

* ```java
  class Solution {
      public ListNode removeNthFromEnd(ListNode head, int n) {
          ListNode first = head, second = head;
          for (int i = 0; i < n; i++) {
              first = first.next;
          }
          if (first == null)
              return head.next;
          
          while (first.next != null) {
              first = first.next;
              second = second.next;
          }
          second.next = second.next.next;
          return head;
      }
  }
  ```

### :star:Q438. [Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

* ```java
  class Solution {
      public List<Integer> findAnagrams(String s, String p) {
          List<Integer> indices = new ArrayList<>();      
          if (p.length() > s.length())
              return indices;
          
          Map<Character, Integer> freq = new HashMap<>();
          for (char c : p.toCharArray()) {
              freq.put(c, freq.getOrDefault(c, 0) + 1);
          }
  
          int l = 0, r = 0;
          char[] c = s.toCharArray();
          while (r < p.length()) {
              updateMap(freq, c[r], freq.getOrDefault(c[r], 0) - 1);
              r++;
          }
          while (r < c.length) {
              if (freq.isEmpty()) 
                  indices.add(l);
              updateMap(freq, c[l], freq.getOrDefault(c[l], 0) + 1);
              l++;
              updateMap(freq, c[r], freq.getOrDefault(c[r], 0) - 1);
              r++;
          }
          if (freq.isEmpty())
              indices.add(l);
  
          return indices;
      }
  
      private void updateMap(Map<Character, Integer> freq, char c, int f) {
          if (f == 0) 
              freq.remove(c);
          else
              freq.put(c, f);
      }
  }
  ```

### Q567. [Permutation in String](https://leetcode.com/problems/permutation-in-string/)

* Variant of Q438

* ```java
  class Solution {
      public boolean checkInclusion(String p, String s) { 
          if (p.length() > s.length())
              return false;
          
          int cnt = 0;
          Map<Character, Integer> freq = new HashMap<>();
          for (char c : p.toCharArray()) {
              freq.put(c, freq.getOrDefault(c, 0) + 1);
          }
  
          int l = 0, r = 0;
          char[] c = s.toCharArray();
          while (r < p.length()) {
              updateMap(freq, c[r], freq.getOrDefault(c[r], 0) - 1);
              r++;
          }
          while (r < c.length) {
              if (freq.isEmpty()) 
                  cnt++;
              updateMap(freq, c[l], freq.getOrDefault(c[l], 0) + 1);
              l++;
              updateMap(freq, c[r], freq.getOrDefault(c[r], 0) - 1);
              r++;
          }
          if (freq.isEmpty())
              cnt++;
  
          return cnt != 0;
      }
  
      private void updateMap(Map<Character, Integer> freq, char c, int f) {
          if (f == 0) 
              freq.remove(c);
          else
              freq.put(c, f);
      }
  }
  ```

### Q643. [Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)

* ```java
  class Solution {
      public double findMaxAverage(int[] nums, int k) {
          int i = 0, j = k - 1, maxSum = 0;
          while (i <= j) {
              maxSum += nums[i++];
          }
          i = 1;
          j++;
          int sum = maxSum;
          while (j < nums.length) {
              sum = sum - nums[i - 1] + nums[j];
              if (sum > maxSum)  
                  maxSum = sum;
              i++;
              j++;
          }
          return maxSum * 1.0 / k;
      }
  }
  ```

***

#### Others

### Q21. [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

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

### :star:Q23. [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

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

### :star:Q160. [Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

* ```java
  public class Solution {
      public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
          ListNode a = headA, b = headB;
          int lenA = 1, lenB = 1;
          while (a.next != null) {
              a = a.next;
              lenA++;
          }
          while (b.next != null) {
              b = b.next;
              lenB++;
          }
  
          if (a != b)     return null;
  
          a = headA;
          b = headB;
          if (lenA >= lenB) {
              for (int i = 0; i < lenA - lenB; i++)
                  a = a.next;
          } 
          else {
              for (int i = 0; i < lenB - lenA; i++)
                  b = b.next;
          }
          while (a != b) {
              a = a.next;
              b = b.next;
          }
          return a;
          
      }
  }
  ```

* Concat two lists: a + b = b + a 

  * No intersection means c1 is a null node

* ```java
  public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
      ListNode a = headA;
      ListNode b = headB;
  
      while(a != b) {
          if(a.next == null)
              a = headB;
          else
              a = a.next;
  
          if(b.next == null)
              b = headA;
          else
              b = b.next;
      }
      return b;
  }
  ```

### Q328. [Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/)

* ```java
  class Solution {
      public ListNode oddEvenList(ListNode head) {
          if (head == null || head.next == null || head.next.next == null)
              return head;
          
          ListNode even = new ListNode();
          ListNode eCopy = even;
          ListNode odd = new ListNode();
          ListNode cur = head;
          int i = 1;
          while (cur != null) {
              if (i % 2 != 0) {
                  odd.next = cur;
                  odd = odd.next;
              }
              else {
                  even.next = cur;
                  even = even.next;
              }
              cur = cur.next;
              i++;
          }
          odd.next = eCopy.next;
          even.next = null;
          return head;
      }
  }
  ```

### :star:Q2181. [Merge Nodes in Between Zeros](https://leetcode.com/problems/merge-nodes-in-between-zeros/)

* **Take advantage of the past irrelevant nodes memory** to avoid creation of new nodes

* ```java
  class Solution {
      public ListNode mergeNodes(ListNode head) {
          int sum = 0;
          ListNode curr = head.next;
          ListNode n = head;
          while (curr != null) {
              if (curr.val == 0) {
                  n.val = sum;
                  sum = 0;
                  if (curr.next != null)
                      n = n.next;
                  else
                      n.next = null;
              }
              else
                  sum += curr.val;
              curr = curr.next;
          }
          return head;
      }
  }
  ```

### Q283. [Move Zeroes](https://leetcode.com/problems/move-zeroes/)

* ```java
  class Solution {
      public void moveZeroes(int[] nums) {
          int first0 = 0, cur = 0;
          while (cur < nums.length) {
              if (nums[cur] != 0) {
                  if (cur > first0) {
                      nums[first0] = nums[cur];
                      nums[cur] = 0;
                  }
                  first0++;
              }
              cur++;
          }
      }
  }
  ```

### Q392. [Is Subsequence](https://leetcode.com/problems/is-subsequence/)

* ```java
  class Solution {
      public boolean isSubsequence(String s, String t) {
          if (s.length() > t.length())
              return false;
  
          char[] sub = s.toCharArray();
          int j = 0, len = sub.length;
          for (char c : t.toCharArray()) {
              if (j >= len)    return true;
              else if (sub[j] == c)    j++;
          }
          return j >= len;
      }
  }
  ```

* Follow up

  * The idea here is to use prefix sum and binary search
  * `O(K*N) -> O(K*MLogN)`

### Q443. [String Compression](https://leetcode.com/problems/string-compression/)

* ```java
  class Solution {
      public int compress(char[] c) {
          int p = 0, cur = 1, num = 1;
          while (cur < c.length) {
              if (c[cur] == c[p]) {
                  num++;
              }
              else {
                  if (num == 1)
                      c[++p] = c[cur];
                  else {
                      char[] digits = String.valueOf(num).toCharArray();
                      for (char d : digits) 
                          c[++p] = d;
                      c[++p] = c[cur];
                      num = 1;
                  }
              }
              cur++;
          }
          if (num > 1) {
              char[] digits = String.valueOf(num).toCharArray();
              for (char d : digits) 
                  c[++p] = d;
          }
          return p + 1;
      }
  }
  ```

### Q1047. [Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)

* ```java
  class Solution {
      public String removeDuplicates(String s) {
          char[] tokens = s.toCharArray();
          int pos = -1, i = 0;
          while (i < tokens.length) {
              if (pos < 0 || tokens[i] != tokens[pos]) {
                  tokens[++pos] = tokens[i++];
              }
              else {
                  pos--;
                  i++;
              }
          }
          return new String(tokens, 0, pos + 1);
      }
  }
  ```

* Or use a stack

***

## Stack

#### Basic push and pop

### :star:Q71. [Simplify Path](https://leetcode.com/problems/simplify-path/)

* ```java
  class Solution {
      public String simplifyPath(String path) {
          String[] paths = path.split("/");
          Deque<String> sp = new ArrayDeque<>();
          for (String s : paths) {
              switch (s) {
                  case "":
                  case ".":
                      break;
                  case "..":
                      if (!sp.isEmpty()) {
                          sp.pop();
                      }
                      break;
                  default:
                      sp.push(s);
              }
          }
          StringBuilder simplePath = new StringBuilder();
          while (!sp.isEmpty()) {
              simplePath.append("/").append(sp.removeLast());
          }
          if (simplePath.length() == 0)   
              simplePath.append("/");
          return simplePath.toString();
      }
  }
  ```

### Q150. [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)

* ```java
  class Solution {
      public int evalRPN(String[] tokens) {
          Deque<Integer> result = new ArrayDeque<>();
          int tmp1 = 0, tmp2 = 0;
          for (String t : tokens) {
              try {
                  int num = Integer.parseInt(t);
                  result.push(num);
              } catch (NumberFormatException e) {
                  tmp1 = result.pop();
                  tmp2 = result.pop();
                  switch (t) {
                      case "+":
                          result.push(tmp2 + tmp1);
                          break;
                      case "-":
                          result.push(tmp2 - tmp1);
                          break;
                      case "*":
                          result.push(tmp2 * tmp1);
                          break;
                      case "/":
                          result.push(tmp2 / tmp1);
                          break;
                      default :
                  }
              }
          }
          return result.pop();
      }
  }
  ```

### Q155. [Min Stack](https://leetcode.com/problems/min-stack/)

* Store the minimum value in each node

* ```java
  class MinStack {
  	private Node head;
          
      public void push(int x) {
          if (head == null) 
              head = new Node(x, x, null);
          else 
              head = new Node(x, Math.min(x, head.min), head);
      }
      
      public void pop() {
          head = head.next;
      }
      
      public int top() {
          return head.val;
      }
      
      public int getMin() {
          return head.min;
      }
          
      private class Node {
          int val;
          int min;
          Node next;
              
          private Node(int val, int min, Node next) {
              this.val = val;
              this.min = min;
              this.next = next;
          }
      }
  }
  ```

### Q225. [Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/)

* ```java
  class MyStack {
      Deque<Integer> queue = new ArrayDeque<>();
      int top;
      int size = 0;
  
      public MyStack() {
          
      }
      
      public void push(int x) {
          queue.addLast(x);
          top = x;
          size++;
      }
      
      public int pop() {
          for (int i = 1; i < size; i++) {
              top = queue.removeFirst();
              queue.addLast(top);
          }
          size--;
          return queue.removeFirst();
      }
      
      public int top() {
          return top;
      }
      
      public boolean empty() {
          return size == 0;
      }
  
  }
  ```

### Q232. [Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/)

* ```java
  class MyQueue {
      Deque<Integer> stack1 = new ArrayDeque<>();
      Deque<Integer> stack2 = new ArrayDeque<>();
  
  
      public MyQueue() {
          
      }
      
      public void push(int x) {
          stack1.push(x);
      }
      
      public int pop() {
          if (stack2.isEmpty()) {
              while (!stack1.isEmpty())
                  stack2.push(stack1.pop());
          }
          return stack2.pop();
      }
      
      public int peek() {
          if (stack2.isEmpty()) {
              while (!stack1.isEmpty())
                  stack2.push(stack1.pop());
          }
          return stack2.peek();
      }
      
      public boolean empty() {
          return stack1.isEmpty() && stack2.isEmpty();
      }
  }
  ```

### Q682. [Baseball Game](https://leetcode.com/problems/baseball-game/)

* ```java
  class Solution {
      public int calPoints(String[] operations) {
          ArrayList<Integer> record = new ArrayList<>();
          int sum = 0;
          for (String s : operations) {
              switch (s) {
                  case "+" -> record.add(record.get(record.size() - 1) + record.get(record.size() - 2));
                  case "D" -> record.add(record.get(record.size() - 1) * 2);
                  case "C" -> record.removeLast();
                  default  -> record.add(Integer.parseInt(s));
              }
          }
          for (int r : record) {
              sum += r;
          }
          return sum;
      }
  }
  ```

### Q735. [Asteroid Collision](https://leetcode.com/problems/asteroid-collision/)

* Simulation

* ```java
  class Solution {
      public int[] asteroidCollision(int[] asteroids) {
          Deque<Integer> stars = new ArrayDeque<>();
          for (int a : asteroids) {
              if (a > 0)
                  stars.push(a);
              // a < 0
              else {
                  if (stars.isEmpty() || stars.peek() < 0)
                      stars.push(a);
                  else {
                      int absolute = Math.abs(a);
                      if (stars.peek() < absolute) {
                          while (!stars.isEmpty() && stars.peek() > 0) {
                              int right = stars.pop();
                              if (right == absolute) {
                                  absolute = 0;
                                  break;
                              }
                              else if (right > absolute) {
                                  stars.push(right);
                                  absolute = 0;
                                  break;
                              }
                          }
                          if (absolute != 0)
                              stars.push(a);
                      }
                      else if (stars.peek() == absolute)
                          stars.pop();
                  }
              }
          }
          int[] arr = new int[stars.size()];
          for (int i = 0; i < arr.length; i++) {
              arr[i] = stars.removeLast();
          }
          return arr;
      }
  }
  ```

### :star:Q946. [Validate Stack Sequences](https://leetcode.com/problems/validate-stack-sequences/)

* ```java
  class Solution {
      public boolean validateStackSequences(int[] pushed, int[] popped) {
          ArrayList<Integer> stack = new ArrayList<>();
          int up = 0;      // pointer to the first unpushed element
          for (int pop : popped) {
              // if stack is empty, all elements remaining on the push stack are pushed
              if (stack.isEmpty()) {
                  while (pushed[up] != pop) {
                      stack.add(pushed[up++]);
                  }
                  up++;
              }
              // an element is ready to be popped
              else {
                  // check the push stack
                  int i = stack.size() - 1;
                  while (i >= 0 && stack.get(i) != pop) {
                      i--;
                  }
                  // if not found, generate the push stack
                  if (i == -1) {
                      while (pushed[up] != pop) {
                          stack.add(pushed[up++]);
                      }
                      up++;
                  }
                  // if found, it should be last element (first to be popped) on the push stack
                  else if (i == stack.size() - 1)
                      stack.removeLast();
                  
                  else
                      return false;
              }
          }
          return true;
      }
  }
  ```

* Rebuild the process

* ```java
  class Solution {
    public boolean validateStackSequences(int[] pushed, int[] popped) {
      Deque<Integer> stack = new ArrayDeque<>();
      int i = 0; // popped's index
  
      for (int x : pushed) {
        stack.push(x);
        while (!stack.isEmpty() && stack.peek() == popped[i]) {
          stack.pop();
          ++i;
        }
      }
  
      return stack.isEmpty();
    }
  }
  ```

### Q1598. [Crawler Log Folder](https://leetcode.com/problems/crawler-log-folder/)

* ```java
  class Solution {
      public int minOperations(String[] logs) {
          int count = 0;
          for (String l : logs) {
              switch (l) {
                  case "../"  -> {
                      if (count != 0)
                          count--;
                  } 
                  case "./"   -> {}
                  default     -> {
                      count++;
                  }
              }
          }
          return count;
      }
  }
  ```

### Q2390. [Removing Stars From a String](https://leetcode.com/problems/removing-stars-from-a-string/)

* ```java
  class Solution {
      public String removeStars(String s) {
          StringBuilder sb = new StringBuilder();
          for (char c : s.toCharArray()) {
              if (c == '*') {
                  if (!sb.isEmpty()) 
                      sb.deleteCharAt(sb.length() - 1);
              }
              else
                  sb.append(c);
          }
          return sb.toString();
      }
  }
  ```



***

#### Valid parentheses

### Q20. [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

* ```java
  class Solution {
      public boolean isValid(String s) {
          // length can not be divided by 2: false
          if (s.length() % 2 != 0)
              return false;
          
          Deque<Character> stack = new ArrayDeque<>();
          for (char c : s.toCharArray()) {
              switch (c) {
                  case '(', '{', '[' -> stack.push(c);
                  case ')', '}', ']' -> {
                      if (stack.isEmpty())
                          return false;
                      else {
                          char prev = stack.pop();
                          switch (prev) {
                              case '(' -> {
                                  if (c != ')')   return false;
                              }
                              case '{' -> {
                                  if (c != '}')   return false;
                              }
                              case '[' -> {
                                  if (c != ']')   return false;
                              }
                          }
                      }
                  }
              }
          }
          return stack.isEmpty();
      }
  }
  ```

### Q921. [Minimum Add to Make Parentheses Valid](https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/)

* ```java
  class Solution {
      public int minAddToMakeValid(String s) {
          int a = 0, b = 0;
          for (char c : s.toCharArray()) {
              if (c == '(')   a++;
              else {
                  if (a != 0) a--;
                  else        b++;
              }
          }
          return a + b;
      }
  }
  ```

### Q1021. [Remove Outermost Parentheses](https://leetcode.com/problems/remove-outermost-parentheses/)

* ```java
  class Solution {
      public String removeOuterParentheses(String s) {
          int count = 0, start = 0, end = 0;
          StringBuilder sb = new StringBuilder();
          char[] c = s.toCharArray();
  
          for (int i = 0; i < c.length; i++) {
              if (count == 0) {
                  if (start < end)    sb.append(s, start, end);
                  start = i + 1;
              }
              if (c[i] == '(') {
                  count++;
              }
              else {
                  count--;
                  if (count == 0)     end = i;
              }  
          }
          if (start < end)    sb.append(s, start, end);
          return sb.toString();
      }
  }
  ```

***

#### Encoding & Decoding

### :star:Q394.  [Decode String](https://leetcode.com/problems/decode-string/)

* st1 maintains a result stack that for each k[encoded_string] pair (or the beginning of the input string) there is a different StringBuilder including the partial or complete result previous to it. Whenever the loop encounter a '[', we push that sb into st1 to store that information

* ```java
  class Solution {
      public String decodeString(String s) {
          Deque<Integer> nums = new ArrayDeque<>();
          Deque<StringBuilder> results = new ArrayDeque<>();
          StringBuilder n = new StringBuilder();
          StringBuilder cur_r = new StringBuilder();
  
          for (char c : s.toCharArray()) {
              if (c >= '0' && c <= '9') {
                  n.append(c);
              }
              else if (c == '[') {              
                  nums.push(Integer.parseInt(n.toString()));
                  n.setLength(0);
                  encodeds.push(cur_r);
                  cur_r = new StringBuilder();
              }
              else if (c == ']') {
                  cur_r.repeat(cur_r, nums.pop() - 1);
                  cur_r = encodeds.pop().append(cur_r);
              }
              else
                  cur_r.append(c);
          }
  
          return cur_r.toString();
      }
  }
  ```

### Q856. [Score of Parentheses](https://leetcode.com/problems/score-of-parentheses/)

* Same idea with Q394: maintain a previous result stack

* ```java
  class Solution {
      public int scoreOfParentheses(String s) {
          Deque<Integer> score = new ArrayDeque<>();
          int cur_s = 0;
          for (char c : s.toCharArray()) {
              if (c == '(') {
                  score.push(cur_s);
                  cur_s = 0;
              }
              else {
                  if (cur_s == 0) 
                      cur_s = score.pop() + 1;
                  else
                      cur_s = cur_s * 2 + score.pop();
              }
          }
          return cur_s;
      }
  }
  ```

#### Monotonic Stack



***

## Queue

### Q933. [Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/)

* ```java
  class RecentCounter {
      Deque<Integer> p;
  
      public RecentCounter() {
          p = new ArrayDeque<Integer>();
      }
      
      public int ping(int t) {
          p.addLast(t);
          while (p.getFirst() < t - 3000) {
              p.removeFirst();
          }
          return p.size();
      }
  }
  ```



***

## HashTable

### Q1207. [Unique Number of Occurrences](https://leetcode.com/problems/unique-number-of-occurrences/)

* ```java
  class Solution {
      public boolean uniqueOccurrences(int[] arr) {
          Map<Integer, Integer> o = new HashMap<>();
          for (int i : arr) {
              if (o.containsKey(i)) 
                  o.put(i, o.get(i) + 1);
              else
                  o.put(i, 1);
          }
          boolean[] ar= new boolean[1001];
          for(int i: o.values())
          {
              if(ar[i])
                  return false;
              ar[i]= true;
          }
          return true;
      }
  }
  ```

### Q1657. [Determine if Two Strings Are Close](https://leetcode.com/problems/determine-if-two-strings-are-close/)

* ```java
  class Solution {
      public boolean closeStrings(String word1, String word2) {
          int m = word1.length(), n = word2.length();
          if(m != n)  return false;
  
          int[] freq1 = new int[26];
          int[] freq2 = new int[26];
          for (char ch : word1.toCharArray()) {
              freq1[ch - 'a']++;
          }
          for (char ch : word2.toCharArray()) {
              freq2[ch - 'a']++;
          }
          
          for (int i = 0; i < 26; i++) {
              if ((freq1[i] == 0 && freq2[i] != 0) || (freq1[i] != 0 && freq2[i] == 0)) {
                  return false;
              }
          }
          Arrays.sort(freq1);
          Arrays.sort(freq2);
          for (int i = 0; i < 26; i++) {
              if (freq1[i] != freq2[i]) {
                  return false;
              }
          }
          return true;
      }
  }
  ```

### Q2215. [Find the Difference of Two Arrays](https://leetcode.com/problems/find-the-difference-of-two-arrays/)

* ```java
  class Solution {
      public List<List<Integer>> findDifference(int[] nums1, int[] nums2) {
          Set<Integer> n1 = new HashSet<>();
          Set<Integer> n2 = new HashSet<>();
          List<List<Integer>> r = new ArrayList<>();
  
          for (int n : nums1)
              n1.add(n);
          for (int n : nums2)
              n2.add(n);
  
          Iterator<Integer> i = n1.iterator();
          while (i.hasNext()) {
              int n = i.next();
              if (n2.contains(n)) {
                  i.remove();
                  n2.remove(n);
              }
          }
          r.add(new ArrayList<Integer>(n1));
          r.add(new ArrayList<Integer>(n2));
  
          return r;
      }
  }
  ```

### Q2352. [Equal Row and Column Pairs](https://leetcode.com/problems/equal-row-and-column-pairs/)

* ```java
  class Solution {
      public int equalPairs(int[][] grid) {
          int len = grid.length;
          if (len == 1)   return 1;
          
          Map<String, Integer> row = new HashMap<>();
          StringBuilder sb = new StringBuilder();
          for (int i = 0; i < len; i++) {
              sb.setLength(0);
              for (int j = 0; j < len; j++) {
                  sb.append(grid[i][j]).append(',');
              }
              row.put(sb.toString(), row.getOrDefault(sb.toString(), 0) + 1);
          }
          int cnt = 0;
          sb.setLength(0);
          for (int j = 0; j < len; j++) {
              sb.setLength(0);
              for (int i = 0; i < len; i++) {
                  sb.append(grid[i][j]).append(',');
              }
              cnt += row.getOrDefault(sb.toString(), 0);
          }
          return cnt;
      }
  }
  ```

***

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

## Recursion

### :star:Q880. [Decoded String at Index](https://leetcode.com/problems/decoded-string-at-index/)

* ```java
  class Solution {
      public String decodeAtIndex(String s, int k) {
          char[] c = s.toCharArray();
          return decode(c, k);
      }
  
      private String decode(char[] c, long k) {
          long curLength = 0;
          long[] length = new long[100];
          int p = 0;
  
          for (int i = 0; i < c.length; i++) {
              if (c[i] >= 'a' && c[i] <= 'z')
                  curLength++;
              else
                  curLength *= c[i] - '0';
              length[i] = curLength;
  
              if (curLength >= k) {
                  p = i;
                  break;
              }
          }
  
          // base case
          if (c[p] >= 'a' && c[p] <= 'z')
              return String.valueOf(c[p]);
          else {
              k %= length[p - 1];
              if (k == 0)
                  k = length[p - 1];
              return decode(c, k);
          }
      }
  }
  ```

* 

***

## Dynamic Programming

### Q1823. [Find the Winner of the Circular Game](https://leetcode.com/problems/find-the-winner-of-the-circular-game/)



***

## Greedy Algorithm

### Q1717. [Maximum Score From Removing Substrings](https://leetcode.com/problems/maximum-score-from-removing-substrings/)

* ```java
  class Solution {
      public int maximumGain(String s, int x, int y) {
          char[] arr = s.toCharArray();
          int score = 0;
          if (x >= y) {
              int[] result = deleteAB(arr, arr.length);
              score += x * result[0];
              result = deleteBA(arr, result[1]);
              score += y * result[0];
          }
          else {
              int[] result = deleteBA(arr, arr.length);
              score += y * result[0];
              result = deleteAB(arr, result[1]);
              score += x * result[0];
          }
          return score;
      }
  
      private int[] deleteAB(char[] arr, int len) {
          int p = -1, cur = 0, cnt = 0;
          while (cur < len) {
              if (p >= 0 && arr[cur] == 'b' && arr[p] == 'a') {
                  p--;
                  cur++;
                  cnt++;
              }              
              else
                  arr[++p] = arr[cur++];
          }
          return new int[] {cnt, p + 1};
      }
  
      private int[] deleteBA(char[] arr, int len) {
          int p = -1, cur = 0, cnt = 0;
          while (cur < len) {
              if (p >= 0 && arr[cur] == 'a' && arr[p] == 'b') {
                  p--;
                  cur++;
                  cnt++;
              }              
              else
                  arr[++p] = arr[cur++];
          }
          return new int[] {cnt, p + 1};
      }
  }
  ```

* 





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

### 
