---
layout: post
title: Getting Started with Github Pages
---

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

Layout and Design Ideas
-------------------------
[Tom Preson-Werner](http://tom.preston-werner.com/) ([github repo](https://github.com/mojombo/tpw)) co-founder of Github and creater of Jekyll

[Poole](http://getpoole.com)([github repo](https://github.com/poole/poole)) simple clean jekyll template

Workflow
--------
Now that things are setup, here's a basic workflow for publishing new posts.

* in `_posts` directory, create new file `YYYY-MM-DD-title.md`
* add [YAML](yaml.org) front matter block at the top of the file
{% highlight yaml %}
---
layout: post
title: Blogging Like a Hacker
---
{% endhighlight %}

* Write something informative (this will probably include a lot of google searches on markdown) then test with Jekyll
{% highlight bash %}
jekyll serve
{% endhighlight %}
* Make sure everything looks good by opening browser to [http://127.0.0.1:4000/](http://127.0.0.1:4000/)

* Push changes to Github
{% highlight bash %}
git commit
git push
{% endhighlight %}
* Open browser to [http://davidhollenberger.github.io](http://davidhollenberger.github.io)
