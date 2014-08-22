---
layout: default
title: "Parsing Emails with Griddler"
date: 2014-07-09
categories: griddler
---

Here are some of the guidelines I wish I found when installing the Griddler gem for parsing emails.

First, install both [Griddler](http://www.griddler.io) and the [Sendgrid](http://sendgrid.com) adapter gem.
You want to make a free sendgrid account. Give them good info because they will be approving your 'developer' account so that
you can send and receive emails from their system.

Set up your [host and url info](https://sendgrid.com/developer/reply) for your Sendgrid account. You're host info will be

    sendgrid-username.bymail.in

and url info should be

    http://yourhostedsite.com/email_processor

<i>Note: I used [ngrok](https://ngrok.com/) to host the my rails server and make it available to receive email in this
dummy app to mess around with Griddler.</i>

At this point, if you send an email to

    this-could-be-anything@sendgrid-username.bymail.in

your app will receive a JSON post request. For all
the steps on what to do next Griddler does have some good [documentation](https://github.com/thoughtbot/griddler) for what to do get the routes set up, and what you can do with that email once it hits your app. If you want to check to see if the email is getting POSTed to your app, then take a look at the rails server logs.
