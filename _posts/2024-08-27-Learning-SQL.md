---
layout: post
title: Learning SQL.
date: 2024-08-27 #13:00:00 -06:00
---
I'm doing a SQL course on Udemy!  [It's the one that's ranked really high.](https://www.udemy.com/course/the-complete-oracle-sql-certification-course)  At first I thought it was too slow, but I bought [Oracle's SQL book,](https://www.amazon.com/dp/1259585492?ref=ppx_yo2ov_dt_b_fed_asin_title) which the instructor of the course recommended, and it turns out the videos I've watched so far cover like 4 chapters of the book already.  That, along with the interview prep exercises I found on leetcode, should be plenty to make me feel prepared for the exam.  In this post I'd like to talk about some neat stuff I'm learning.

## Ampersand substitution

This is a SLQ*Plus feature, but according to the book, it will be on the 1Z0-071 exam.  It looks like it's covered in video 66 in the course but so far I've only read about it in the book.

Basically the idea is you can create batch files in SQL.  From context, I'm gathering that a batch file is any file with code in it that you can ask your compiler to run.  However when I Google "What is a batch file?" the results I get seem to mainly refer to files with Windows command line code in them.  You may want to write a batch file if you have certain code that you want to reuse, with slight modifications.  In SQL, as in many languages, you can create a prompt asking the user what to change.  The ampersand allows you to define a variable, that you can then set to a value, given a prompt, with the `ACCEPT` command.

```SQL
ACCEPT vRoomNumber PROMPT "Enter a room number: "
SELECT ROOM_NUMBER, STYLE, WINDOW
FROM SHIP_CABINS
WHERE ROOM_NUMBER = &vRoomNumber;
```

This script can be run for different room numbers we want to query.  `PROMPT` tells the user to pick one and `ACCEPT` stores what the user typed as the value of the variable.  The variable for the room number is given the name `vRoomNumber` (I'm not sure why the book uses a 'v' at the beginning of the name, other than to distinguish it as a variable, 'v' for variable).  To call the variable in the query you need the ampersand.

The ampersand also gives you a prompt without needing to use `ACCEPT`.  Given the following code:

```SQL
SELECT ROOM_NUMBER, STYLE, WINDOW
FROM SHIP_CABINS
WHERE ROOM_NUMBER = &RoomNumber;
```

The output will be the prompt, `Enter value for roomnumber:` and what the user types will be stored as RoomNumber.  Notice there is no 'v' here.  (Also, I'm aware SQL is not a case-sensitive language but I'm still not sure whether that applies to variable names.  The prompt may actually be `Enter value for RoomNumber:`.)

## `SOUNDEX()`

This is a character function that returns what a string "sounds like".  It is a code with a letter followed by three digits, based on the first several letters of the word.  The letter is just the first letter of the word.  The digits that follow are encoded as follows:

**Letter**             |  **SOUNDEX Code**
-----------------------|------------------
B, F, P, V             |  1
C, G, J, K, Q, S, X, Z |  2
D, T                   |  3
L                      |  4
M, N                   |  5
R                      |  6
A, E, H, I, O, U, W, Y |  - ignored -

I'm actually not sure what happens if the code is less than three digits long, but I tried Googling around.  I think maybe 0s are added.  So for example, "Wheeles" (the last 'e' is not pronounced) would be W420.  

I'm sure this algorithm is not perfect, but it still seems pretty useful.  The best example of its use is in querying a database of names where some names are the same but spelled differently.
