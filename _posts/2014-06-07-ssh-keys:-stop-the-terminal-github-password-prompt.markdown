---
layout: default
title: "SSH Keys: Stopping 'Github Password' Prompt"
date: 2014-06-08
categories: terminal
---

<header class="post-header">
<h1>{{ page.title }}</h1>
<p class="meta">{{ page.date | date: "%b %-d, %Y" }}</p>
</header>

<article class="post-content">
<p>
There is an easy way to improve your git to github workflow. When making new repo's or interacting with Github in Terminal, you'll often get a prompt that looks something like this:

{% highlight ruby%}
Github username:
Github password:
{% endhighlight %}
</p>
<p>
You're pushing your files to Github via 'https' which is the Github default. By changing this process to run using 'SSH Keys' you can stop the annoying prompt and speed up your workflow./p>
<p>
Check out the link for step by step <a href="https://help.github.com/articles/generating-ssh-keys">instructions</a>.
</p>
</article>
