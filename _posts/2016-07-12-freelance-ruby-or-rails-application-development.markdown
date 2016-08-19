---
layout: post
comments: true
title: 'Freelance full stack Ruby on Rails application development'
author: Krzysztof Maicher
date: 2016-07-12 09:11:04
categories:
tags: [ruby, rails, freelance, angular, angularjs, application, web application, software development]
---


### Introduction

I took a few days off from my regular work to create an application for an employment agency located in Holland. I would like to present here, how I’ve approached this task. I invite you to read!

### Background

The client was an employment agency, that organizes work for people in agricultural sector. They also provide accommodation for their employees during the time of their employment. The client wanted to be able to monitor gas and electricity consumption in those places.
They already had meters installed (a kind of [Smart Meters](https://en.wikipedia.org/wiki/Smart_meter)) at those places. These meters were performing __readings__ every few minutes and collecting them as __files on FTP server__.
It was hard for the client to view and manage these reading, so the client was looking for a solution, that would view all these data in a more friendly form, like __tables and charts__.
They also wanted to group these data by addresses, perform some comparisons, see daily and hourly usage in selected date ranges and have costs per person automatically calculated.
They contacted me and asked to propose a solution.

### The solution

Currently, I specialize in building web application using Ruby on Rails and it seemed like a __Ruby on Rails web application__ could satisfy client's requirements.
I proposed following solution, consisting of three parts:

- An admin panel with classic CRUD for managing addresses and settings (Ruby on Rails, PostgreSQL).
- A script, acquiring data from ftp server (Ruby, CRON).
- Minimalistic frontend with a nice tables and charts (HTML/CSS, Bootstrap3, JavaScript/CoffeeScript with some AngularJS inserts for better UX).

I contacted the client to discuss each of above part once again. I wanted to check, if I understood their requirements.
We agreed on [time & material](http://c2.com/cgi/wiki?TimeAndMaterialContract) [payment model](https://blog.lelonek.me/how-to-charge-our-clients-ade8b6a53102#.tr023ghm4) and I presented hourly estimates for each of above part, to get them a sens of costs.
I declared to contact them at the end of each part to show the progress and to discuss some potential changes or misunderstandings.

### Work process

For my own convenience, I decided to keep the code quality high.
I mean to keep the code decoupled, write tests, pay attention to data integrity (validations, foreign keys, unique indexes), handle edge cases, follow the unofficial Ruby style guide and use Rubocop to check it.

Keeping the code quality high and a [craftsmanship approach](http://thecleancoder.blogspot.com/2010/09/hacker-novice-artist-and-craftsman.html) helped me go through development process efficiently.

### Results

I used following technology stack, to build the application:

- Ruby 2.2
- Rails 4.2
- PostgreSQL 9.4
- AngularJS 1.4
- Bootstrap 3

Here are some screens of the finished app:

{% include image.html url="gt_dashboard.jpg" description="Bar charts showing gas and electricity cost for each address in selected date range" %}
{% include image.html url="gt_charts_days.jpg" description="Line charts showing daily electricity consumption for selected addresses and date range" %}
{% include image.html url="gt_charts2_hours.jpg" description="Line charts showing hourly electricity consumption for selected addresses and one day" %}
{% include image.html url="gt_meters_index.jpg" description="Meters table" %}
{% include image.html url="gt_address_index.jpg" description="Addresses table" %}
{% include image.html url="gt_address_edit.jpg" description="Address edit form" %}
{% include image.html url="gt_login.jpg" description="Admin login form" %}
{% include image.html url="gt_stats.jpg" description="Statistics" %}

Here are other articles related to that application:

- [My approach to a database design process for Rails application](/my-approach-to-a-database-design-process-for-rails-application.html)
