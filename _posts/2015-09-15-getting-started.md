---
layout: post
title: Getting Started with Github Pages
comments: True
---

Why use Github Pages and Jekyll?
--------------------------------
* I'm relatively new to Github, so it seems like a good way to learn more about it
* Learn markdown, again because it's used by Github and it seems like a great way to document things.

Setting up Github Pages
-----------------------

Here are the basic steps I followed for setting up Github Pages.  For additional information see [Github Pages](https://pages.github.com/).

1. Create a repository called username.github.io.
2. Clone [lanyon](https://github.com/poole/lanyon) repo and copy files to your new repository.

    ```bash
    git clone https://github.com/poole/lanyon.git
    cd lanyon
    cp * <path_to_github_pages_repo>
    ```

3. Create a directory called ```_drafts``` in your Github Pages repo.  This will serve as a staging area for new blog posts that you do not want published.  

4. Install [Jekyll](https://http://jekyllrb.com/).  Jekyll will allow you to test your changes locally before pushing them to Gihub.  

    ```bash
    gem install jekyll
    ```  


Workflow
--------
Now that things are setup, here's a basic workflow for publishing new posts.

1. In `_posts` directory, create new file `YYYY-MM-DD-title.md`
2. Add [YAML](yaml.org) front matter block at the top of the file

    ```yaml
    ---
    layout: post
    title: Blogging Like a Hacker
    ---
    ```

3. Write something informative (this will probably include a lot of google searches on markdown) then test with Jekyll

    ```bash
    jekyll serve
    ```

4. Make sure everything looks good by opening browser to [http://127.0.0.1:4000/](http://127.0.0.1:4000/)

5. Push changes to Github

    ```bash
    git commit
    git push
    ```

6. Open browser to *username*.github.io

Layout and Design Ideas
-------------------------
I like the simplicity that this format allows.  I borrowed most of the design of the page from the following places.

[Tom Preson-Werner](http://tom.preston-werner.com/) ([github repo](https://github.com/mojombo/tpw)) co-founder of Github and creater of Jekyll

[Lanyon](http://lanyon.getpoole.com)([github repo](https://github.com/poole/lanyon)) jekyll template

Add Custom URL
--------------

After getting comfortable with the basic setup and workflow of Github pages I wanted to add a custom URL for the site.  Here are the steps I followed to setup my domain registered with [Namecheap](www.namecheap.com).

1. Login to your account, select **Domain List** and click **Manage** next to the domain you would like to configure.
2. Click **Advanced DNS** and add the following host records:
  * Type: *A Record*  Host: *@*  Value: *192.30.252.153*  TTL: *30 min*
  * Type: *A Record*  Host: *@*  Value: *192.30.252.154*  TTL: *30 min*
  * Type: *CNAME Record* Host: *www* Value: *username.github.io.*  TTL: *30 min*
3. Save all changes.
4. Navigate to your custom URL.  Your blog page should load just like it did when navigating directly to *username*.github.io.


Closing Thoughts
------------------------

Overall I'm pretty happy with this setup.  Considering I'm not a web guy and have never setup my own blog before, I feel like this will give me a good foundation to build on. Some things I would like to add down the road are: Enabling comments, Linking to Twitter/other social media, Search and better Tagging, and Google Analytics.  I'm sure there will always be something else to tweak and play around with.  Most importantly, this is a great way to get blogging and share information with my co-workers and peers.

###Update
I added Twitter and comments to my blog.  [Joshua Land](http://joshualande.com/jekyll-github-pages-poole/) has a great write up on how to do this.

I'm still very happy with Github Pages and how my blog has progressed.  There's so much flexibility with this platform and it's so easy to add content to new posts.
