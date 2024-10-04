---
layout: post
title: Testing sorting algorithms.  
---
Gilbert & Forouzan recommends testing sorting algorithms with four types of arrays:

__Test Case__ | __Sample Data__
----------|------------
Random data ... | .. {5, 23, 7, 78, 22, 6 19, 33, 51, 11, 93, 31}
Nearly ordered ... | .. {5, 6, 7, 21, 19, 22, 23, 31, 29, 33, 51, 93}
Ascending ordered ... | .. {5, 6, 7, 11, 19, 22, 23, 31, 33, 51, 78, 93}
Descending ordered ... | .. {93, 78, 51, 33, 31, 23, 22, 19, 11, 7, 6, 5}

Once I am satisfied with my sorting algorithms study page I want to write a C program that tests them all.  I just noticed F&G even has a test drive template for insertion sort.  

My C samples for sort algorithms are so far heavily based on those in F&G.  In each case the algorithm is not the main program but a function within it.  Thus I learned something today, when I was wondering why on Earth F&G's functions would take two aruments, the array and the array's size.  I learned C does not have an attribute for an array that gives its size.  I saw some workarounds on [stackexchange.com](https://stackoverflow.com/questions/4081100/c-finding-the-number-of-elements-in-an-array).  A common solution was to use <code>sizeof()</code>.  The <code>sizeof()</code> function gives the _memory_ size of its input (I don't remember if it's in bits or bytes).  So given an array _A_, one can do something like:
```c
size_t array_size_A; 
  /* size_t type is used since sizeof() can return 
     large, but unsigned, numbers */
array_size_A = sizeof(A)/sizeof(A[0]);
```
But then there's a catch!  You can't make a function that does this -- when passing an array to a function C only passes the array's address.  In other words, upon its input in a function the array _A_ becomes a pointer, to the address of _A_.  If an array is defined globally but passed to any function, then <code>sizeof()</code> won't work within that function, either, for the same reason.

Another workaround involves signalling the end of an array with <code>NULL</code>.  Then to find the number of elements in an array, count them.  But ugh, it looks like the convention is to just pass the array along with its size.  TS.

Bleh!  The table in the preview window looks much nicer than the one on the actual post webpage.  I'm guessing I need to appeal to html in order to properly pad it so it doesn't look like ass.  For now I just put some ellipses to make it easier to read.  Also, in my programs I've started naming variables using underscores and functions using the capital letter for each new word (as in Macaulay2).  I hope that's a good convention.
