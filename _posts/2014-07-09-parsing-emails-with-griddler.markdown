---
layout: default
title: "Parsing Emails with Griddler"
date: 2014-07-09
categories: griddler
---

<header class="post-header">
<h1>{{ page.title }}</h1>
<p class="meta">{{ page.date | date: "%b %-d, %Y" }}</p>
</header>

<article class="post-content">
<p>
Here are some of the guidelines I wish i found when installing the Griddler gem for parsing emails.
</p>

<p>
First, install both <a href="http://www.griddler.io">Griddler</a> and the <a href="http://sendgrid.com">Sendgrid</a> adapter gem.
You want to make a free sendgrid account. Give them good info because they will be approving your 'developer' account so that
you can send and receive emails from their system.
</p>
<p>
Set up your <a href="https://sendgrid.com/developer/reply">host and url info</a> for your Sendgrid account. You're host info will be
{% highlight ruby %}sendgrid-username.bymail.in{% endhighlight %} and url info should be {% highlight ruby %}http://yourhostedsite.com/email_processor{% endhighlight %}.
<i>Note: I used <a href="https://ngrok.com/">ngrok</a> to host the my rails server and make it available to receive email in this 
dummy app the mess around with Griddler.</i>
</p>
<p>
At this point, if you send an email to {% highlight ruby %}anything@sendgrid-username.bymail.in{% endhighlight %} your app will receive a JSON post request. For all
the steps on what to do next Griddler does have some good <a href="https://github.com/thoughtbot/griddler">documentation</a> for what to do get the routes set up, and what you can do with that email once it hits your app. If you want to check to see if the email is getting POSTed to your app, then take a look at the rails server logs.
</p>
</article>
