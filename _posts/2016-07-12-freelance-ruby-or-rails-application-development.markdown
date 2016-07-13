---
layout: post
comments: true
title: 'Freelance Ruby on Rails application development'
author: Krzysztof Maicher
date: 2016-07-12 09:11:04
categories:
tags: [ruby, rails, freelance, angular, angularjs, application, web application, software development]
---


### Introduction

I took a few days off from my regular work to create an application for an employment agency located in Holland. I would like to present here, how I’ve approached this task. I invite you to read!

### Background

The client was an employment agency, that organizes work for people in agricultural sector. They also provide accommodation for their employees during the time of their employment. The client wanted to be able to monitor gas and electricity consumption in those places.
They already had meters installed at those places. These meter were performing __readings__ every few minutes and collecting them as __files on FTP server__.
It was hard for the client to view and manage these reading, so the client was looking for a solution, that would view all these data in a more friendly form, like __tables and charts__.
They also wanted to group these data by addresses, perform some comparisons, see daily and hourly usage in selected date ranges and have costs per person automatically calculated.
They contacted me and asked to propose a solution.

### The solution

Currently, I specialize in building web application using Ruby on Rails and it seemed like a __Ruby on Rails web application__ could satisfy client's requirements.
I proposed following solution, consisting of three parts:

- An admin panel with classic CRUD for managing addresses and settings (Ruby on Rails, PostgreSQL).
- A script, acquiring data from ftp server (Ruby, CRON).
- Minimalistic frontend with a nice tables and charts (HTML/CSS, Bootstrap3, JavaScript with some AngularJS inserts).

I contacted the client to discuss each of above part once again. I wanted to check, if I understood their requirements.
We agreed on [time & material](http://c2.com/cgi/wiki?TimeAndMaterialContract) [payment model](https://blog.lelonek.me/how-to-charge-our-clients-ade8b6a53102#.tr023ghm4) and I presented hourly estimates for each of above part, to get them a sens of costs.
I declared to contact them at the end of each part to show the progress and to discuss some potential changes or misunderstandings.

### Work process

For my own convenience, I decided to keep the code quality high.
I mean to keep the code decoupled, write tests, pay attention to data integrity (validations, foreign keys, unique indexes), handle edge cases, follow the unofficial Ruby style guide and use Rubocop to check it.

Keeping the code quality high and a [craftsmanship approach](http://thecleancoder.blogspot.com/2010/09/hacker-novice-artist-and-craftsman.html) helped me go through development process efficiently.

### Results

In future posts I will try to show some code snippets.

Here are some screens of the finished app.
