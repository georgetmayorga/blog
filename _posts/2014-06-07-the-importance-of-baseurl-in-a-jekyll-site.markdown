---
layout: default
title:  "The Importance of 'baseurl' in a Jekyll Site"
date:   2014-06-06
categories: jekyll update
---
<header class="post-header">
<h1>{{ page.title }}</h1>
<p class="meta">{{ page.date | date: "%b %-d, %Y" }}</p>
</header>
<p>The first thing to share is the lesson learned while launching this blog using [Jekyll][Jekyll]. 
After installing the gem, building the site, and getting it up and running on localhost I was ready to push my site to the gh-pages branch I made for it. When I did this I ran into a problem.
</p>
<p>
My index.html page was not accessing the CSS file, and what was showing up looked nothing like my website. On top of that, the links to my posts were leading to 404 errors. I googled, looked at the documentation but no luck.
</p>
<p>
I asked someone with Jekyll experience and a fresh set of eyes to look at the issues. They cloned my repo, and started problem solving knowing that it wasn't the site but had something to do with the way it was being read by Github. Turns out that the .yml file that sets up all the configuration in Jekyll was missing the "baseurl" information. Turns out there is some documentation [here][here] about how to configure the baseurl. Just make sure it looks like this within the quotes: "username.github.io/project-name/"
</p>
[Jekyll]: http://jekyllrb.com
[here]: http://jekyllrub.com/docs/github-pages/#project-page-url-structure

