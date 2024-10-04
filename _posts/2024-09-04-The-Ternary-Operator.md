---
layout: post
title: The ternary operator.
date: 2024-09-04 #13:00:00 -06:00
---
When I first bought my JavaScript book [(JavaScript: The Definitive Guide by David Flanagan)](https://www.amazon.com/JavaScript-Definitive-Most-Used-Programming-Language-dp-1491952024/dp/1491952024/ref=dp_ob_title_bk), I read like the first ten chapters of it at once.  At one point, I read about something called the _ternary operator_, given by `?:`.  At the time I didn't understand exactly what its use was, but I've since seen it again and decided to look into it.

In mathematics, a ternary operator is any function that takes in three inputs and outputs one result.  An example is distributivity of addition and multiplication: 

$$f(x,y,z) = (x+y)z = xz + yz$$ 

is an example that takes in three inputs (numbers $x$, $y$, and $z$) and returns one output (the number $xz+yz$).

In computer languages, the ternary operator generally refers to one thing, the _if-else_ statement.  In fact, and this wasn't clear in my JavaScript book when I was reading about it, the ternary operator is just shorthand notation for an if-else statement.  In other words, the ternary operator condenses the statement

```javascript
if (a === 4) {
    b = b + 1;
} else {
    a = a + 1;
}
```

which adds 1 to `b` if `a` is equal to 4 and adds 1 to `a` otherwise, to: 

```javascript
a === 4 ? b = b + 1 : a = a + 1
```

I read about this when I was browsing the internet about "idiomatic Python".  In Python, there is no ternary operator, but the [site I went to](https://medium.com/the-andela-way/idiomatic-python-coding-the-smart-way-cc560fa5f1d6#id_token=eyJhbGciOiJSUzI1NiIsImtpZCI6ImE0OTM5MWJmNTJiNThjMWQ1NjAyNTVjMmYyYTA0ZTU5ZTIyYTdiNjUiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJodHRwczovL2FjY291bnRzLmdvb2dsZS5jb20iLCJhenAiOiIyMTYyOTYwMzU4MzQtazFrNnFlMDYwczJ0cDJhMmphbTRsamRjbXMwMHN0dGcuYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJhdWQiOiIyMTYyOTYwMzU4MzQtazFrNnFlMDYwczJ0cDJhMmphbTRsamRjbXMwMHN0dGcuYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJzdWIiOiIxMTIxNzY0MzAwOTgzMDE0MjMzMjEiLCJlbWFpbCI6ImxleWpmazZAZ21haWwuY29tIiwiZW1haWxfdmVyaWZpZWQiOnRydWUsIm5iZiI6MTcyNTA0MjQ2MywibmFtZSI6IkFzaGxleSBLLiBXLiBXYXJyZW4iLCJwaWN0dXJlIjoiaHR0cHM6Ly9saDMuZ29vZ2xldXNlcmNvbnRlbnQuY29tL2EvQUNnOG9jS3dsd1RrbXFjdlpreXg2R0IzZ1oxQzlKLVE0WGhpamlBSlp0OEhnc2pzbm55YTlVaWN3dz1zOTYtYyIsImdpdmVuX25hbWUiOiJBc2hsZXkgSy4gVy4iLCJmYW1pbHlfbmFtZSI6IldhcnJlbiIsImlhdCI6MTcyNTA0Mjc2MywiZXhwIjoxNzI1MDQ2MzYzLCJqdGkiOiIwNzI5MTRhMjFjYzk4YThkZDZjNGEyZWViODVkMmE3OGQ4ZTE1NzcyIn0.YtnvaTdikPEfAUjiwtiea4hednxbvR4FHEE1hNpQhOFV-n8kb6fATA1ebPUhOnVc_ZXrt7geMRT8qIdS2g7z9aJNkwS8n18QgVSYcyd7GdErjiJAqLeObu0ccS1M5aDsjZbe8IeYWOdwj-7saUKuHScui_KS5Dq9_d3FkgDSXIXFfVTCIM-pWLDzzC2oLZ8uSnju9l1RsyBavetVlbvATrKCbPOQxSdwB11p4fCTYgGeMUKxaBfsSBW6oYhvEool2Jnmo2m561TP2GCQzzA1cl9i058i9WUJ0x8uXbxSkrw0bU-KpKuGNG27KRHnPbi5x-Tnb97J9gWf7ZbW2prqtw) says the idiomatic way to make a ternary operator in Python is to take a statement like:

```python
a = True
value = 0
if a:
  value = 1
print(value)
```

and replace it with:

```python
a = True
value = 1 if a else 0
print(value)
```

(In JavaScript this statement would be `a = True; a === True ? value = 1 : value = 0; console.log(value)`.)

[Wikipedia describes the ternary operator in mathematics,](https://en.wikipedia.org/wiki/Ternary_operation) as well as its incarnation [in various programming languages.](https://en.wikipedia.org/wiki/Ternary_conditional_operator)  One example I'm familiar with is in SQL -- the `CASE` command is actually a generalization of the ternary operator.  It is not a ternary operator but a $(2n+1)$-ary operator, in that it takes an odd number of inputs and returns one output:

```SQL
SELECT (CASE WHEN a > b THEN x WHEN a < b THEN y ELSE z END)
AS CONDITIONAL_EXAMPLE
FROM tab;
```

In pseudocode, the expression in the parentheses is "if a > b then x else if a < b then y else z", or, in JavaScript, `a > b ? x : a < b ? y : z`.  So in this case it's a $5$-ary operator!

I seem to remember reading about this in the context of idiomatic JavaScript in my book, but it was hard to find where I read about it.  Nice that I looked this up, though, and now I understand it.