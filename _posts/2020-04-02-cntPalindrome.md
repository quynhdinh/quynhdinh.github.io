---
layout: post
title: "2 solutions to the counting palindromic substrings problem"
---

Recently, I met this problem.
> **Given a string S. Count the number of substring of S which is a palindrome**.
> A `substring` is a contiguous sequence of characters within a string(a character is still considered as a substring). A `palindrome` is a string which is the same when reading forward and backward.


Here I share two solutions to this problem.
You can submit your solution [here](https://leetcode.com/discuss/interview-question/algorithms/125095/count-palindromes)
# Dynamic Programming solution O(N ^ 2)
We define `dp[i][j] = 0` if the substring `s[i..j]` is a palindrome and `1` otherwise.

We will fill in this table.

First, every character is a palindrome.

Second, every string of 2 have the same character is a palindrome.

Finnaly, if `s[i..j]` is a palindrome iif `s[i] == s[j]` and `s[i + 1..j - 1]` is a palindrome.

The answer is the number of `1` in the dp.
```python
def countPalindromicSubstrings(self, s):
        n = len(s)
        
        dp = [[0 for i in range(n)] for j in range(n)]
        
        for i in range(n):
            dp[i][i] = 1
        answer = n # we have n palindromes already
        for col in range(1, len(s)):
            for row in range(col):
                if row == col - 1 and s[col] == s[row]: 
                    dp[row][col] = 1
                    answer += 1
                elif dp[row + 1][col - 1] == 1 and s[col] == s[row]:
                    dp[row][col] = 1
                    answer += 1
        
        return answer
```
Here, we use an 2d array and 2 loop to fill in this table so the time and space complexity of this approach are both O(N ^ 2).
# O(1) space solution
In coding interview, the interviewer will probably ask you to improve the perfomance up to using O(1) space (i.e: create no extra space).
Iterate over the array and treat each element as a center of a palindromic substring then try to expand to the left and to the right of this center. Every time `s[left] == s[right]` we increase the answer by one and `break` when we see `s[left] != s[right]`. A while loop will help us to do this.
 
```python
def countPalindromicSubstrings(self, s):
        n = len(s)
        answer = 0
        for mid in range(n):
            left = right = mid # count for odd substrings
            while left >= 0 and right < n and s[left] == s[right]:
                left -= 1
                right += 1
                answer += 1
            left = mid
            right = mid + 1 # count for even substrings
            while left >= 0 and right < n and s[left] == s[right]:
                left -= 1
                right += 1
                answer += 1
        return answer
```
Here, the time complexity is still O(N ^ 2). But we only need to use an extra variable `answer` to store the answer. Hence, the space complexity is O(1).