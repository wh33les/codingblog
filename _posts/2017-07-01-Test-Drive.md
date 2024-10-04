---
layout: post
title: Test drive! 
---
As an exercise, go back to the [10 June 2017 post](https://wh33les.github.io/Structures-in-C/) and zoom in until the screen capture of my C snippet illustrating an array becomes readable.  Assuming the all-caps comments are filled in with correct code, or filling them in yourself, what do you expect will be the result of running this little program?  I could've sworn I'd already tested this using an online compiler but yesterday I found out I was wrong.

Since I've been working on my Unix proficiency, I downloaded [Cygwin](https://www.cygwin.com/) and the [gcc](https://gcc.gnu.org/) package(s) to compile this code.  
>Cygwin is a Unix-like environment and command-line interface for Microsoft Windows. Cygwin is huge and includes most of the Unix tools and utilities. It also include[s] the commonly-used Bash shell.
<br> _-- from <a href="https://www3.ntu.edu.sg/home/ehchua/programming/index.html">yet another insignificant programming notes...</a>_

First, a shoutout to the users on this [stackoverflow forum](https://stackoverflow.com/questions/21922469/how-do-i-assign-values-to-an-array-using-scanf-within-a-for-loop) whose snippets were helpful in debugging my thing.  Nonetheless, it took me awhile to find the mistake.  I also learned while writing the portion that validates the user input, that C has no way to return the type of a variable.  And so, while I can ensure the user's input of an integer is between 0 and 9 inclusive, I can't ensure the input is actually an integer.

<!--<img src="https://wh33les.github.io/images/seasonedC.png" title="seasoned C" height="100%" width="100%">-->
![Seasoned C](./images/seasonedC.png)

I called this file test-drive.c.  My intent is to rework this little program into a function among others that illustrate various data structures and sorting algorithms.  The main program shall consist of the actual tests, maybe with user prompts.  Go ahead and test this one!  

I also had a little fun in refining my "coding voice", meaning, my commenting style, my use of tabs and spaces for keeping things organized, my choice of loops, etc.  The lovely syntax highlighting is from [Notepad++](https://notepad-plus-plus.org/), my text editor of choice for some years now.  [This blogger](https://codedvisions.blogspot.com/2014/12/opening-files-in-notepad-from-cygwin.html) gave the most concise instructions I could find for invoking npp from within a Cygwin terminal (and I finally got around to asking the web how to navigate between windows with keyboard shortcuts so now I don't have to wear out my finer finger muscles in using the touchpad to move and click the mouse arrow).

If you're interested <sup id="a1">[[1]](#f1)</sup>, [these instructions](https://web.cecs.pdx.edu/~pkwong/ECE103_files/Resources/Compiler/C_GNU/GCC_Installation/How_to_Install_Cygwin+GCC.htm) will get you going with Cygwin and the correct packages for using gcc.  I found it helpful after installing Cygwin to put the setup-x86.exe file in the root directory Cygwin makes on your computer (the default is C:\cygwin but you might have changed it).  Then, when you want to install more packages later you can do so directly from the Cygwin terminal: type `cd /` and press enter (puts you in the root directory), then type `run setup-x86` and press enter.  It will look like you are completely reinstalling Cygwin but just click through the wizard prompts until you get back to the "Select Packages" window.  There are, of course, [other quickhacks of doing this](https://github.com/transcode-open/apt-cyg) but this way works for me now.  To use gcc, [this page](https://www3.ntu.edu.sg/home/ehchua/programming/cpp/gcc_make.html#zz-1.3) can get you started.  It also has instructions for using gcc with MinGW, but ignore those if you prefer using Cygwin.     

<sup id="f1"><a href="https://stackoverflow.com/questions/25579868/how-to-add-footnotes-to-github-flavoured-markdown">[1]</a></sup> There is a bias toward Windows users here, you may or may not have noticed. [[back]](#a1)
