---
layout: post
title: Structures in C.
---
I am working on a page that lists each of the following data structures
* array list/vector
* linked list
* queue
* stack
* hashtable
* binary search tree
* priority queue/heap

along with an example in C, pros/cons, when to use, how to implement, big-O properties of basic operations, e.g., find, add, remove.  This exercise is based on a suggestion by @jw0 as part of my preparation for a Google interview.  Much of the information above I'm learning from Chapter 3 of Skiena's _The Algorithm Design Manual, 2nd Ed._  

Yesterday I wrote a C snippet illustrating an array.  In it, comments in all caps represent unseen blocks of code that are not the emphasis of the example.  The snippet prompts a user for 10 digits between 0 and 9, and then prints them as an array.
<!--<img src="https://wh33les.github.io/images/naiveCExampleOfArray.png" title="naïve C" class="wrap align-left" height="60%" width="60%">-->
![Na&#239;ve C](./images/naiveCExampleOfArray.png)
In retrospect this snippet seems pretty naïve, now that I've today refamiliarized myself with the <code>typedef</code> command, and the idea of structures.  My primary reference for C has been my old textbook from college, _Computer Science: A Structured Programming Approach Using C, 2nd Ed._ by Forouzan & Gilberg.  I took this course in the spring of 2006 so needless to say, I'm pretty rusty on C.  Evidently we did cover structures at the end of the course.  I totally didn't remember this until I saw all the blocks of highlighted text (this is how I "took notes" back then) when I opened the book.  Granted, we covered pointers about halfway through the course, which I do remember, in that I remember not understanding what the hell the point is of using pointers.  

Since Skiena conveniently writes examples of algorithms in C, I've had a chance to compare styles between the two texts.  Here is Skiena's example of a singly-linked list:
```C
typedef struct list {
  item_type item;        /* data item */
  struct list *next;     /* point to successor */
} list;
```
F&G describe three ways to declare or define a structure:
1. Variable structures
```C 
struct { field-list } variable_identifier;
```
This method is the most limited because it can only be used for one variable definition; i.e., it is not really a type.  The way I understand this, it means one is really just defining an array that can have varied data types in its entries.

2. Tagged structures
```C
struct tag { field-list } variable_identifier;
```
Here, I think the tag is what really makes a structure a newly defined data type, called <code>struct tag</code> (with whatever the tag is actually named, for example, "linked-list"), just as <code>int</code> and <code>char</code> are data types.  Then the variable_identifier is the name assigned to a given structure with that tag.

3. Type-defined structures
```C 
typedef struct { field-list } TYPE-ID;
```
This is the most powerful and recommended method of declaring a structure.  I think the distinction here is that while <code>TYPE-ID</code> is now in the role of <code>tag</code>, the new data type is just called <code>TYPE-ID</code> (again, whatever that type-id is actually named, e.g., "LINKED_LIST", and convention says the name is in all caps)... Otherwise, I don't see any other distinction from the tagged structure method.  Here, the variable_identifier is not included, which then makes the tagged structure method seem redundant.  F&G also says the tagged and typedef methods can be combined into a tagged typed definition... So what exactly is the point of using a tag at all?

Of course, this means in Skiena's example, the definiton of a singly-linked list includes the tag "list" and the type-id "list".  I am not sure how to apply this template to a more concrete example I can give in my data structure study page.

On another note, I figured out how to change my avatar on the homepage for this blog.  On all other pages though, the picture does not show up.  I haven't yet figured out why.
