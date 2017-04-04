---
layout: post
comments: true
title: 'Animated Spinner for Ruby Command Line'
author: Krzysztof Maicher
date: 2016-11-23 18:30:00
categories:
tags: [ruby, rubygems]
---

## Introduction

[__CliSpinnable__](https://github.com/maicher/cli_spinnable) is a gem that enriches Ruby CLI by:

- spinner
- tick mark (&#10003;)
- fail mark (&#215;)

## What for?

In command line programs it is always nice to se information describing what is happening at the moment.

This information ensures us, that the program isn't hanging.

When building command line program, you can choose outputting some logs.

Sometimes, on long tasks without any meaningful logs, it makes sense to show animated spinner.

Then _CliSpinnable_ can be handy.

## How to use?

__CliSpinnable__ gem doesn't require any configuration.
It makes it easy to use, but in the same time it's not very flexible and it will suit only to specific use cases, such as described in demo section.

It's main feature is `CliSpinnable.with_spinner` method.

Call it with the block and then use `#print`, `#tick` and `#fail` methods: 

{% gist maicher/eec6436a5e171e91108304736f943642 %}

Can also be mixed-in to the class:

{% gist maicher/c5e09d6b088276a701fed47d0ca7fecd %}

## Idea

Lat's say, that you are building a CLI script in Ruby, that downloads some data and then processes it.

Each of those two steps can take few seconds to execute. 

Each of them can either fail or succeed, so it would be nice to be able to mark them with a: 

 - success mark (&#10003;) when step will succeed, 
 - or fail mark (&#215;) followed by error when something will fail.

## Demo

{% gist maicher/405a10faebfe31f9f2225805d15dc3f7 %}

__Output:__

![demo.git](https://raw.githubusercontent.com/maicher/cli_spinnable/master/examples/demo.gif)

You can check out more examples [here](https://github.com/maicher/cli_spinnable/tree/master/examples).

## Feedback

Since I constantly learn Ruby and OOP, your feedback is welcome.

Feel free to checkout and analyze the gem source_code, you can find there:

- asynchronous writing to `STDOUT` using `Thread` and `Queue`
- `Refine`/`Using` applied on `String` for adding colors to outputted text
- `Enumerable#cycle` for infinitive spinner rotation
- tests using Rspec
