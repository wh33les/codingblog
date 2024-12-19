---
layout: post
title: Subdirectory. 
---
I am trying to change the url of my blog but I can't figure it out.  I followed the directions for Solution 2 on [this page](https://github.com/jbranchaud/blog/blob/master/_posts/2013-03-02-Running-Your-Jekyll-Blog-from-a-Subdirectory.md) but it didn't work.  I also edited the \_config.yml file by changing the url and baseurl.  I also noticed in that file I have mathjax set to true, so I don't know why I've also had problems rendering LaTeX in my blog posts.  

Now I just reloaded my site and all the formatting went away!  I read Solution 1, but it said to tell the deployment scheme to deploy the blog directory and I don't know how to do that.  But I don't understand why Solution 2 doesn't work.  OK, now I changed everything back and it loads correctly.  I think I had to change permalink back to :title.html.  OK, now I changed permalink to /blog/:title.html and nothing happened, but then when I changed it back /blog shows up in the url of my blog posts now.  But the homepage for the blog is still not under /blog.  I just changed url to https://..., I noticed there was no s in the name.  Doesn't work.  

OK now I committed the updates to this post and tried to load it and got a 404 page.  Same thing happened when I clicked on another post.  But when I remove /blog from the url it loads.  However, when I click on a post it puts /blog in the url and I get the 404 page.  As far as I know everything in the \_config.yml file is back to the way it was.  I just tried changing permalink back to /blog/:title.html.  Somehow, that fixed it, but the url for the blog post no longer contains /blog.

Maybe I should try playing around with the other files in the repository.  I wish I understood how Jekyll compiles this blog.  Wait, now I updated this post and tried to click on it and got the 404 page again.  Same thing happened when I tried to click on another blog post.  But when I put /blog back in the url it loads.  Looking at the \_config.yml file again and I see that I didn't change url back to what it was before.  I changed it back, and changed permalink back to :title.html.  Now I think the \_config.yml file is back to where it was originally.  That fixed everything.

_Update:_  I did it!  All I had to do was change this repository name to Blog and put "/Blog" for baseurl in the \_config.yml file.  I think that's it?  I don't remember doing anything else. 
