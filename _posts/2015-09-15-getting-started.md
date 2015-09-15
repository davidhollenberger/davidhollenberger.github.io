---
layout: post
title: Getting Started with Github Pages
---

{{ page.title }}
================
<p class="meta">15 Sep 2015</p>

Why use Github Pages and Jekyll?
--------------------------------
* I'm relatively new to Github, so it seems like a good way to learn more about it
* Learn markdown, again because it's used by Github and it seems like a great way to document things.
* It's free!

Setting up Github Pages
-----------------------
* Check out the [Github Pages](https://pages.github.com/) and follow the instructions.  I am starting out with just a User site.

* Install [Jekyll](https://http://jekyllrb.com/)
```
gem install jekyll
```  

Let me be honest, I am not a web developer.  I do not have much interest in learning CSS/HTML or any kind of web design right now.  Thankfully, Jekyll has a few example blogs on their site and links to the Github repos of each blog.  One of the blogs listed is [Tom Preson-Werner](http://tom.preston-werner.com/) ([github repo](https://github.com/mojombo/tpw))who is a co-founder of Github and creater of Jekyll.  I "borrowed" most (ok, all) of the layout and design of this blog from him.

Workflow
--------
Now that things are setup, here's a basic workflow for publishing new posts.

* in `_posts` directory, create new file `YYYY-MM-DD-title.md`
* add [YAML](yaml.org) front matter block at the top of the file

```
---
layout: post
title: Blogging Like a Hacker
---
```

* Write something informative (this will probably include a lot of google searches on markdown) then test with Jekyll

```
jekyll serve
```
* Make sure everything looks good by opening browser to [http://127.0.0.1:4000/](http://127.0.0.1:4000/)

* Push changes to Github

```
git commit
git push
```
* Open browser to [http://davidhollenberger.github.io](http://davidhollenberger.github.io)
