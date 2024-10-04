---
layout: post
title: Access codes and Python.
date: 2024-10-04 #13:00:00 -06:00
---
As promised, a post about the access code I had to enter for this semester's data science boot camp.

I thought it would be really neat to learn how Steven did this.  The first thing I did was Google, "how do you create an access code using a jupyter notebook?".  The first result from the AI answer was really good, it just uses the `random` module:

```python
import random

def generate_access_code(length=6):
    """Generates a random access code of the specified length."""
    characters = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    code = "".join(random.choice(characters) for i in range(length))
    return code

access_code = generate_access_code()
print("Your access code is:", access_code)
```

Steven's function does it much differently, though.  I'm parsing it right now.  OK, it takes the first part of the version of `numpy` being used (the part before the first decimal point), then appends the first part of the version of `pandas` being used, then `matplotlib`, then `pyjokes`.  This number is called $z$.  The function then creates a $100\times 3$ array from a .txt file of numbers between $-1$ and $1$, up to several decimal places.  Then it fits a linear regression model with $X$ as the first two columns and $y$ as the last column.  It mulitplies the regression coefficients rounded to 2 decimal places by $z$ and then rounds the result to an integer.  That's the code. 

Steven answered my inquiry as to how he validated the codes and he answered just as I was finishing parsing his function.  You may have noticed, anyone who follows Steven's instructions for installing Anaconda correctly should get the same number for $z$.  Steven even clarified, to ensure everyone set up the `conda` environment correctly he had it use an old version of a module called `pyjokes` that people weren't likely to already have.  Then, the linear regression was just to test that Python was working correctly.

So the point is everyone should have gotten the same versions of the modules and thus the same $z$, and if everything was working correctly they should've gotten the same regression coefficients, and thus the same code -- and that's how he verified it!  Way simpler than I was expecting.  Actually, I guess I thought there was a Python package that could do all this.  According to Google (at least the first result, see below), there isn't.  

As for verifying a randomly generated code, Google has different ways of doing it, but here's how I would do it, using Google's AI's function above to generate the code.  I would create a file with different seeds and have my function choose one at random to generate a random number for the code.  Then, I guess I would have another file with the resulting codes.  Then when a user enters a code, validate it by checking to see if it's in the file.  I guess it doesn't even have to be that complicated, I could just create a file with a certain number of randomly generated numbers.  Then in the function that generates the code, choose one of those numbers randomly.

So this really is simpler than I was expecting.  As for the security of the codes?  That's a different story.  Even Steven said, "Certainly cryptography has a lot to say about secret code generation."  

I looked it up and it's not a matter of cryptography algorithms, per se.  I found out there is a Python module called `secrets` that generates "truly random" codes.  The codes should be stored in a file, as in my idea, but encrypt the file.  There's a module called `bcrypt` that can do this.  Use external measures to protect the database of codes, such as more encrytpion and access controls.  Limit validation attempts from the same IP address (I think there's software that can do this).  Use codes that expire after a certain amount of time, I don't know how to do this.  Then implement other internet security measures like https and two-factor authentication.

*Another note:*  In writing this blog post in VS Code, I learned how to open two folders at the same time.  I needed to do it so I could go back and forth between Steven's function, in the cloned DS boot camp repository, and my blog post, in my Blog repository.  To do it just go to File -> Add Folder to Workspace...  You can even save the workspace to reopen later.
