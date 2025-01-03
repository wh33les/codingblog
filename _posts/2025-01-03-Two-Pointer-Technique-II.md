---
layout: post
title: Two-pointer technique II.
date: 2025-01-03 #13:00:00 -06:00
---
[See the last post for an introduction.](https://transitioning-to-industry.org/Two-Pointer-Technique-I.html)  Here are two more applications of the two-pointer technique.

## Two iterables 

In some cases two iterables (e.g., arrays, strings) are given.  If we iterate through each with a separate pointer then we can potentially reduce the time complexity to $O(n)$.  [Here is an example problem:](https://leetcode.com/problems/is-subsequence/description/) Given two strings `s` and `t`, return `true` if `s` is a subsequence of `t`, or `false` otherwise.  My solution beats time complexity on Leetcode, but doesn't do well on space complexity:
```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        if s == '':
            return True
        
        little = 0
        for big in range(len(t)):
            if s[little] == t[big]:
                little += 1
            if little >= len(s):
                return True    
        return False
```
The confusion is in the case where `s` is an empty string, and making sure `little` doesn't exceed `len(s)-1`.  Leetcode's two-pointer solution doesn't run into the problem with the empty string because of the conditional on `LEFT_BOUND`.  It does slightly better on memory:
```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        LEFT_BOUND, RIGHT_BOUND = len(s), len(t)

        p_left = p_right = 0
        while p_left < LEFT_BOUND and p_right < RIGHT_BOUND:
            # move both pointers or just the right pointer
            if s[p_left] == t[p_right]:
                p_left += 1
            p_right += 1

        return p_left == LEFT_BOUND 
```
Note that other strategies for this problem exist that optimize memory usage.  Leetcode's solution above also allows for more generalization of this two-pointer technique, for problems where both iterables must be completely exhausted to complete the problem.  An example where this is necessary is given two sorted integer arrays `arr1` and `arr2`, return a new array that combines both of them and is also sorted.  My solution is slightly modified from Leetcode's (Leetcode just uses a second `while` loop for the case where `arr2` is not exhausted):
```python
def combine(arr1, arr2):
    # ans is the answer
    ans = []
    i = j = 0
    while i < len(arr1) and j < len(arr2):
        if arr1[i] < arr2[j]:
            ans.append(arr1[i])
            i += 1
        else:
            ans.append(arr2[j])
            j += 1
    
    if j < len(arr2):
        arr2 = arr1
        i = j 

    while i < len(arr1):
        ans.append(arr1[i])
        i += 1
     
    return ans
```

## Sliding window

The sliding window variation works well for problems analyzing subarrays of a given array.  One pointer is the index of the beginning of the subarray and the other pointer is the index of the end of the subarray.  The problem gives some condition for a subarray to be "valid" and this condition is checked for each subarray as the "window" is resized and shifted.

Sliding window is much more efficient than the na&iuml;ve approach, which is to enumerate every possible subarray then check each one.  Given an array of size $n$ there are $\binom{n}{2}$ subarrays, making an algorithm to enumerate and check each one at best $O(n^2)$.  With the sliding window, the left pointer can move at most $n$ times and the right pointer can move at most $n$ times, making the traversal $O(n)$.  

Here is a variation of a problem I struggled with before I learned the two-pointer technique, [Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/description/) (I struggled with [Max Consecutive Ones I](https://leetcode.com/problems/max-consecutive-ones/description/) and [II](https://leetcode.com/problems/max-consecutive-ones-ii/description/)): Given a binary array `nums` and an integer `k`, return the maximum number of consecutive 1's in the array if you can flip at most `k` 0's.  Here was my first submission:
```python
class Solution:
    def longestOnes(self, nums: List[int], k: int) -> int:
        left = num_zeroes = longest = 0
        for right in range(len(nums)):
            if nums[right] == 0:
                num_zeroes += 1
            while num_zeroes > k:
                if nums[left] == 0: 
                    num_zeroes -= 1
                left += 1
            longest = max(longest, right-left+1)
        return longest 
```
The two pointers are `left` and `right`.  More generally, one could initialize both `left` and `right` and then create conditionals on when to increment each one.  However, for this problem it's clear that `right` needs to increment at every step so it's the index of the `for` loop.

One more little strategy I've learned that I want to mention.  In this problem I had to find the longest subarray satisfying a certain condition.  In the past I would have made a new array storing the length of each subarray that satisfies the condition, then taken the max of that subarray.  But this takes $O(n^2)$ of memory in worse-case scenario where every subarray is valid.  To avoid this, just store the current max in one variable, `longest`.  At each iteration compare the value that would've been stored in the extra array to the current `longest`.  I'm learning other little ways to optimize memory usage in my algorithms but in general I'm finding it's harder than minimizing time complexity.