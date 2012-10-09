---
layout: post
title: "Wonderful Octopress"
date: 2012-10-09 14:37
comments: true
categories: note
---

Today, I install Octopress, it is wonderful.  
I follow the official docs
[setup](http://octopress.org/docs/setup/)
[deploy](http://octopress.org/docs/deploying/github/)
[config](http://octopress.org/docs/configuring/)
[blogging](http://octopress.org/docs/blogging/)  
and the other two blogs:
[jokry.com](http://jokry.com/blog/2012/03/08/octopress/)
[blog.javachen.com](http://blog.javachen.com/2012/06/migrate-blog-form-wordpress-to-github-with-octopress/)

The following command is in common use(in the project root directory):

    rake new_post["title"]
    rake new_page[super-awesome]
    rake new_page[super-awesome/page.html]

    rake preview
    rake generate
    rake deploy

    git add .
    git commit -m 'your message'
    git push origin source

Add a about page

    rake new_page[about]

Then I modify main navigation, edit
`source/_includes/custom/navigation.html`


    <ul class="main-navigation">
    <li><a href="{{ root_url }}/">Blog</a></li>
    <li><a href="{{ root_url }}/blog/archives">Archives</a></li>
    <li><a href="{{ root_url }}/about">About</a></li>
    </ul>

Also see [theme/template](http://octopress.org/docs/theme/template/)
