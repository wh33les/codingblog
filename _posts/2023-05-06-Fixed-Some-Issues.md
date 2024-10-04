---
layout: post
title: Fixed some issues.
---

The other night I finally figured out what was wrong with the pictures on this blog!  They weren't loading in my posts, and then recently my avatar stopped loading, too.  I had been meaning/trying to fix this for a long time.  It turned out to be pretty easy to fix.  

To fix the avatar, all I had to do was edit the `img` tag on my default.html file to say `<img src="{{ site.baseurl }}/images/{{ site.avatar }}" />`, then on the \_config.yml file set the avatar to the name of the picture, no file path.  At some point I want to learn about the double braces in the file path (they're not showing when I compile the html for this page, I think I need an escape character to see them).  Is that an html thing, or is it specific to Jekyll?  I'd really like to get a better understanding of how Jekyll compiles this blog.

To fix the pictures, it turns out I was just using the wrong file path.  I had been using the entire file path on my pictures, but I needed to use the relative path.  All of my pictures are in a folder in the top directory called "images".  The blog posts are in a folder called "\_posts" so I just needed to use the path `./images/picture`, where `picture` is the name of the picture, with its file extension.  Note the single period to get to the parent directory, not the usual two periods.  Everywhere I'm looking right now with a Google search says to use two periods, but somewhere I found it said to use one, and that's what worked.

Finally, I also figured out how to make my links open in a new tab by default, with exceptions.  I'd already known that to make them open in a new tab by default I just needed to add a base tag to the `head` element of my default.html file, `<base target="_blank">`.  Yesterday morning I found out for the exceptions I just need to add `target="_self"` to the `a` tag.  I did this for the links on my website, too, but I was too lazy to delete the `target="_blank"` from all the individual tags.  Of course they're not hurting anything by being there, but at some point I would like to go through and delete them, since I'm trying to show off the code I wrote.  Then the next thing I want to try to do for my website is create a uniform `head` element for all of my pages, where I can add exceptions for the home page.
