---
layout: post
title: Two-pointer technique I
date: 2025-01-02 #13:00:00 -06:00
---
Been awhile!  After the election forecast I started a Kaggle competition that I am looking to wrap up pretty soon.  But in the meantime I got a job interview, and it's a coding interview.  I scheduled it for the first or second week of February so I would have time to prepare.  A friend of mine at Google said he used leetcode to prepare for his interview so that's where I started.  I went ahead and signed up for Premium because I wanted half off on courses.  Then I found an [Explore Card track for data structures,](https://leetcode.com/explore/featured/card/the-leetcode-beginners-guide/679/sql-syntax/4358/) that I believe comes with Premium already, and began with that.  Turned out to be more challenging than I expected!  Took over 6 hours to get through the first card, and with 19 cards, I knew I wouldn't be able to get through them all in time.  By the second card I was completely lost and suddenly my preparation felt unproductive.  Then my friend told me to look at the *interview* materials on leetcode and I found [this course in data structures](https://leetcode.com/explore/interview/card/leetcodes-interview-crash-course-data-structures-and-algorithms/) that claimed to be an efficient way to prepare for a coding interview.  It is!  And something that came up in the course that had been touched on in the Explore Cards, too, is what's known as the __two-pointer technique__.  

To introduce the two-pointer technique I'll give the example from the Explore Cards, since I struggled with it for so long: Given an integer array `nums` sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same. Then return the number of unique elements in `nums`.  [(Here is the problem.)](https://leetcode.com/problems/remove-duplicates-from-sorted-array/description/)  After several runtime errors about out-of-bounds indices, here is my first wrong answer:
```python
class Solution(object):
    def removeDuplicates(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        for i in range(len(nums)-1):
            if i >= len(nums)-1:
                break
            while nums[i] == nums[i+1]:
                nums.remove(nums[i+1])
                if i+1 >= len(nums)-1:
                    break    
        return len(nums)    
```
The `break` statements are there because removing elements of the array makes the length of the array smaller, and so `i+1` in the `while` loop or in the next iteration of the `for` loop may exceed the length of the new array.  This solution passes 293 test cases then fails when `nums=[1,1,1]` -- the output is `[1,1]`.  (After removing one of the 1s, `i+1` is still `1` but `len(nums)-1=1` and the `while` loop is broken.  Then in the next iteration of the `for` loop `i=1` and is once again too big, so the `for` loop is broken, without removing the other 1 from `nums`.)

In the article following this exercise the two-pointer technique is introduced for this problem.  The idea is to keep track of two different positions in `nums` at the same time.  In this case, since we are modifying the array in-place, one pointer keeps track of where we are in comparing entries in the array, and the other pointer keeps track of where we are modifying the array.  Here is the implementation:
```python
class Solution(object):
    def removeDuplicates(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        insertIndex = 1
        for i in range(1, len(nums)):
            if nums[i - 1] != nums[i]:
                nums[insertIndex] = nums[i]
                insertIndex = insertIndex + 1
        return insertIndex
```
Now the size of `nums` is no longer changing, so we don't get index out-of-bounds errors anymore.  The `insertIndex` pointer is exactly what counts the number of unique entries in the array, which is what the problem asks for.

For what other types of problems is the two-pointer technique useful?  The interview course goes into way more detail, and right away so I didn't have to waste anywhere near as much time struggling with the exercises (I don't think it's possible or reasonable that I would've ever come up with this technique on my own, even though it's considered "basic").  

The first variation of the two-pointer technique is where one pointer starts at the beginning of the array, the other starts at the end, and in a `while` loop a condition is checked and the two pointers are moved closer together until they meet in the middle.  A straightforward example of this technique is in reversing a given string `s`.  [Here is the problem on leetcode,](https://leetcode.com/problems/reverse-string/description/) and here is my solution (I started writing solutions in Python3 when I switched to the crash course):  
```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        beg = 0
        end = len(s) - 1
        while beg < end:
            s[beg], s[end] = s[end], s[beg]
            beg += 1
            end -= 1
        return   
```
Of course, this is just a simple example -- technically, a faster solution is:
```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        for i in range(ceil(len(s)/2)):
            s[i], s[-i-1] = s[-i-1], s[i]
        return 
```        
But in general, two pointers is appealing because with the two pointers covering different parts of the array, as long as the operations in the `while` loop are $O(1)$, the algorithm is $O(n)$.  In particular, it can avoid nested loops, which are $O(n^2)$ at best.  One example of a such a problem is given a sorted array of unique integers, find a pair of entries that sums to a given integer.  

[Here is an example that I really struggled with in the Explore Cards:](https://leetcode.com/problems/squares-of-a-sorted-array/description/) Given an integer array `nums` sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order.  My first submission passed 129 test cases then on `nums=[-9995,-9994,...,9993,9994]` it failed the time constraint:
```python
class Solution(object):
    def sortedSquares(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        squares = [num*num for num in nums]
        sorted = []
        for i in range(len(squares)):
            sorted.append(min(squares))
            squares[squares.index(min(squares))] = max(squares) + 1
        return sorted   
```
The challenge is that some integers are negative and some are positive, so just squaring the entries doesn't necessarily result in a sorted array.  The second line in the `for` loop is meant to change `min(squares)` to the next smallest entry by making the current min the new max.  I'm realizing now this operation costs $O(n^2)$, as it must traverse `squares` to find the max for each entry in `squares`.  Of course after this solution I tried the obvious approach:
```python
class Solution(object):
    def sortedSquares(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        squares = [num*num for num in nums]
        squares.sort()
        return squares   
```                
I also realize now that my identifier `sorted` in the previous solution is a reserved word in Python.  Python's `sort()` methods runs in $O(n\log n)$ time.  Final solution, with the two-pointer technique, beat all my other submissions in speed:
```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        n = len(nums)
        ans = [0]*n
        left, right = 0, n-1
        while left <= right:
            if abs(nums[left]) > abs(nums[right]):
                ans[n-1] = nums[left]*nums[left]
                left += 1
            else:
                ans[n-1] = nums[right]*nums[right]
                right -= 1
            n -= 1    
        return ans 
```
Surprisingly, this solution used the most space, though.  Not sure exactly why, since all three solutions create a new array and should have been $O(n)$ space complexity.

In general, the two-pointer technique is handy for problems dealing with arrays and even more versatile than this.  More examples in the next post, stay tuned!
