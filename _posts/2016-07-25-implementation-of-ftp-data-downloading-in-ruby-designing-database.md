---
layout: post
comments: true
title: 'Implementation of FTP data downloading in Ruby. Part 1: designing database'
author: Krzysztof Maicher
date: 2016-07-25 19:39:04
categories:
tags: [Ruby, Rails, database, PostgreSQL]
---

### Introduction

Some time ago, I've created a Ruby on Rails application, which purpose was to present data (electric and gas meter readings) by charts and tables.
The application was acquiring that data from FTP.

You can read more about that app [here](/freelance-ruby-or-rails-application-development).

In these article I would like to present, how I've approached the database designing process.

### Analyzing the data

Data, that were stored on FTP by meters had following characteristics:

- meter readings in csv files on FTP server
- about 50 lines in each file
- one line represent one reading (two values, timestamp and a meter number)
- around 30 new files a day (can be more in future)
- that gives around 1.5k new readings every day

Over a year, the data would grow to _1.5k x 365_, that is around _0.5M_ records each year.
That is not that much and a properly designed PostgreSQL database can handle that amount of data without any problems.
My idea was to store that data in a relational database, that would allow me to easily access it later to view it as charts or tables.

### Designing database

I decided, that the application will interact with database by ActiveRecord models. I like to use ActiveRecord, because of its ease of use.
However, when it comes to creating new ActiveRecord models, my approach may be different from other Rails developers.
What I seen, that sometimes Rails developers jump into generating models (using `rails generate model ...`) without any thoughts about how the database should look like.
I like to come up with a __database design upfront__ (in mind, on paper or in some tool) and after that, I start generating ActiveRecord models and adjust settings in migrations to satisfy the database design.
In other words I design database at first, and then I build an __ActiveRecord models layer on top of it__.

In this case I came up with following database schema.

![Database schema](/assets/gt_readings_db.png)

I generated that schema using SQL Power Architect tool afterwards. It is not complex, so I could make that design in my head before start generating models.
However, if project needs more tables with more sophisticated relations between them, I could draw that design on paper or in that tool before generating models and migrations.

Btw. during my career I found out that presenting this kind of schemas in projects' documentations helps other developers to understand projects' database design.

Because the tables was generated from Rails, it can be assumed, that they follow all Rails conventions.

How to read that schema?

- There are 3 tables: `meters`, `readings`, `ftp_files` (in application they are represented by models: Meter, Reading, FtpFile).
- Each table has a `id` row, which is a Primary Key (`[PK]`).
- There are various column types: `INTEGER`, `VARCHAR`, `TIMESTAMP`.
- Some columns have a non-null constraint (`[AK]`).
- A [foreign key](https://www.postgresql.org/docs/8.1/static/tutorial-fk.html) is set between meters and readings tables (`[FK]`).

I'd like to ask you, after seeing that, can you create Rails migrations, the result of which would be above schema?

### Unique indexes

What cannot be seen in schema presented above is unique indexes.
I'd like describe them here.

My idea of _ftp_files_ table was to store a list of file names, that have already been parsed.
It's pointless to allow keeping there doubled names. That is why I set a [unique index](https://www.postgresql.org/docs/9.4/static/indexes-unique.html) on the _name_ column.

A _Meter_ is identified by it's _number_.
There has to be only one meter with given number, that's why I decided to apply unique index on it as well.
Having a unique index on that column gives the assurance, that in application, every time a meter will be searched by number, there will be zero or exactly one meter found.

As it goes tor _readings_, it is highly undesirable to double readings in database, because charts created from them would present incorrect values.
One reading consists of 4 data. A _timestamp_, _meter_id_, _value1_, _value2_. How to prevent from saving double readings?
In PostgreSQL a unique index can also be applied to group of columns.
There is a complete certainty, that when application for any reason (bug, my or some other developers mistake, or even a client's software bug) will try to save a double reading, an exception will be raised (from database level), which is very desirable.
I don't want to see double readings in that database. Unique indexes guarantees it and I can rely on that.

### Data integrity

It is highly probable, that that peace of database will keep it own [data integrity](https://en.wikipedia.org/wiki/Data_integrity) regardless of what will happen in application (if of course the application will not modify the schema).

- Meters will not be doubled.
- Readings will not be doubled.
- There will be no readings without a value, timestamp or associated meter.
- There will be no [orphaned records](http://www.dhdursoassociates.com/database-glossary-3.html#orphan) on meters-readings associacion.

Note, that the __security is kept on a database layer__. I other words, the database uses it own mechanisms to prevent loosing its own integrity.

That database doesn't need ActiveRecord layer to keep data integrity, which is desirable, bacause ActiveRecord validations (even though they are very useful and helpful) __doesn't guarantee the application's data integrity__.
That is because ActiveRecords validations can be omitted by using following methods:

`#save(validate: false)`, `.update_attribute`, `delete`, `.delete_all`, `.update_all`

Furthermore someone can modify database tables by raw SQL queries, or even by logging into psql console to run some updates or inserts. Finally, my own code can have bugs and for example can try to create double records.

### Summary

I see database as a layer, that can keep its data consistency by itself, without the use of ActiveRecord models.
I use ActiveRecord often and I think it is a solid piece of library. It speeds up the development process, comparing to not using any ORM system at all.
However using ActiveRecord models without a solidly designed database can lead to frustrations and to lots of maintenance work, when database will start loosing its integrity.

When you aim for a solid database design, learn about foreign keys and start using them.
Start using uniqueness validations not only in ActiveRecord models, but at a database layer as well (including unique indexes on groups or inside json fields).
Start specifying if a column can have null value or not and if not, then specify a default value.
