---
layout: post
comments: true
title: 'Verifying database schema in console'
author: Krzysztof Maicher
date: 2016-06-29 08:24:10
categories:
tags: [ruby, rails, migrations, psql, PostgreSQL, MySQL]
---

In Rails we have a `schema.rb` file, where we can check, how our database looks like.

However, logging in to database console can be sometimes useful.

Here are a list of needed console commands.

### PSQL and MySQL console commands

| __Description__ |__PostgreSQL__ | __MySQL__ |
|---------------|-----------|--------------------- |
| show database list |`\list` | `show databases;` |
| connect to to database |`\connect DBNAME` | `use DBNAME;` |
| show all tables |`\dt` | `show tables;` |
| show table details |`\d+ TABLE` | `describe TABLE;` |
| exit from console |`\q` | `exit;` |

&nbsp;

### How to log into database console?

It depends on the operation system and how the database was installed.

In general:

__PostgreSQL__ | __MySQL__
====|====
`$ psql` | `$ mysql -u root -p`

&nbsp;

### When checking console can be helpful?

Lastly I found myself experimenting in psql console, when I was setting indexes inside psql jsonb fields. I wasn't sure, if Rails migration modified database schema how I expected, so I wanted to double check it in the console.

Other examples can be:

#### To check differences between test and development database

#### To check results of custom SQL in migrations

#### To modify foreign keys or indexes, which names are not Rails default
