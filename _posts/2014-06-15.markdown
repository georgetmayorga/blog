---
layout: default
title: "Primary and Foreign Keys in Postgres"
date: 2014-06-15
categories: sql, postgres
---

<header class="post-header">
<h1>{{ page.title }}</h1>
<p class="meta">{{ page.date | date: "%b %-d, %Y" }}</p>
</header>

<article class="post-content">
<p>
When making my own database in postgres, I had trouble finding explicit documentation on how to create a table with an auto-incrementing primary key, then make a second table with it's own auto-incrementing primary key and a foreign key referencing table 1. I will walk through the steps starting at making a database at your command line:

{% highlight ruby%}
$ createdb my_db                      #creates a database
$ psql my_db                          #launches database
$ CREATE TABLE table1(
  id SERIAL PRIMARY KEY,              #serial makes it auto-incrementing
  name TEXT
  );

$ CREATE TABLE table2(
  id SERIAL PRIMARY KEY,              #don't forget the serial here too
  my_column2 TEXT,
  my_column1 TEXT,
  table1_id INT REFERENCES table1(id)   #REFERENCES table(column) creates foreign_id
  );
{% endhighlight %}
</p>
<p>
With this code you would have made a database called <code>my_db</code>, with 2 tables called <code>table1</code> and <code>table2</code>. You would have then created auto-incrementing unique <code>id</code> columns in each with a foreign key in <code>table2</code> called <code>table1_id</code>.
<p>
For more information head over to the <a href="http://www.postgresql.org/docs/9.0/static/sql-createtable.html">postgres documentation</a> to learn more about creating tables.
</p>
</article>
