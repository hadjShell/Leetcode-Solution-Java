# Leetcode Solution

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

