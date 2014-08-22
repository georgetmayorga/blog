---
layout: post
title: Setting up Paperclip with Amazon S3
---

I just got [PKG.CAT](http://pkg.cat) up and running and one of the biggest 
hurdles I faced was getting it launched on Heroku with Paperclip and S3. It was 
incredibly difficult, the documentation seemed to have holes in the errors I was
 getting. I've collected some information that was helpful to me and am sharing it
below.

[Uploading Files to S3 in Ruby with Paperclip](https://devcenter.heroku.com/articles/paperclip-s3)

This was a GREAT place to start, and why I started off wide-eyed about how easy
this could be. One mistake I made was missing that you must add

    gem 'aws-sdk'

in your Gemfile. Make sure you include it in production.

After I had set up AWS, I ran into one problem that took me a while to figure out.
 Rails was trying to open the Paperclip file through a local path, but now that file 
existed at a URL. Thinking about it now it may seem obvious, but there was NO WHERE 
saying that you need to change how you open your Paperclip file.

[Changing File Path to S3 URL](http://stackoverflow.com/a/15470253)

This is the StackOverflow post that helped me realize what I was doing wrong. I 
hopped in rails console and found out the path I was looking for by testing the methods 
on the Paperclip `file` object. `expiring_url(60)` was not a method that worked for 
me but this was my code after some hacking:

~~~ruby
@package.attachments.each do |attachment|
  attachments[attachment.file_file_name.to_s] = 
    open(attachment.file.url).read
end
~~~

Using `open(attachment.file.url).read` gave me access to the url Mailer is looking 
for to put in the outgoing attachments.
