---
layout: default
title: "Confident Code and Apple Design"
date: 2014-07-29
categories: style
---

<header class="post-header">
<h1>{{ page.title }}</h1>
<p class="meta">{{ page.date | date: "%b %-d, %Y" }}</p>
</header>

<article class="post-content">
<p>
I recently watched a talk <a href="https://twitter.com/avdi" target="_blank">Avdi Grimm</a> gave at a 2011 ruby conference on <a href="http://www.youtube.com/watch?v=T8J0j2xJFgQ" target="_blank">confident code</a>. When talking about it after, I thought a very useful analogy could be found by comparing an class Windows 2-button mouse, and the Apple single-button variety.</p>
<p>
Timid code is brittle and unsure of itself. Through if statements, it asks for internal state (is this object.nil?) not knowing what kind of object it will receive. Objects are being split up into groups (often nil, and not nil) through these if statements. Confident Code instead tells an object what to do rather than asking what is is. The object is acceptable as long as it responds to the methods being called.</p>
<p>Through the Null Object Pattern programmers can make a NullObject class that responds to the same methods being called on the target object. This allows the programmer to remove the if statements and tell an object to respond how it sees appropriate, be it nil or anything else.</p>
<p>I would argue that the reasons programmers are driven to the Null Object design pattern is the same reason we appreciate the Apple single click mouse. Instead of the Windows 2-button mouse, the Apple single button mouse hides the options from the physical design. The options are still there for one menu or the other, the seams are hidden a bit further down helping the aesthetic beauty of the device.</p>
