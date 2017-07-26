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

I wrote [a gem](https://github.com/maicher/cli_spinnable) that enriches Ruby CLI by spinner.

## Why?

Because existing solutions did not fit my use case.
   
I needed:

- spinner
- tick mark (&#10003;)
- fail mark (&#215;)

## Use case

I was building a CLI script in Ruby, that downloads some data and then processes it.

There were few steps, each took few seconds to execute and could either fail or succeed.
I wanted to be able to mark them with a: 

 - success mark (&#10003;) when step succeed, 
 - or fail mark (&#215;) followed by error message when step failed.

## How?

It's written in pure Ruby, using:

- asynchronous writing to `STDOUT` using `Thread` and `Queue`
- `Refine`/`Using` applied on `String` for adding colors to outputted text
- `Enumerable#cycle` for infinitive spinner rotation
- rspec tests

## How to use?

__CliSpinnable__ gem doesn't require any configuration.
It makes it easy to use, but in the same time it's not very flexible and it will suit only specific use cases, such as described above.

It's main feature is `CliSpinnable.with_spinner` method.

Call it with the block and then use `#print`, `#tick` and `#fail` methods: 

{% gist maicher/eec6436a5e171e91108304736f943642 %}

Can also be mixed-in to the class:

{% gist maicher/c5e09d6b088276a701fed47d0ca7fecd %}

## Demo

{% gist maicher/405a10faebfe31f9f2225805d15dc3f7 %}

__Output:__

![demo.git](https://raw.githubusercontent.com/maicher/cli_spinnable/master/examples/demo.gif)

You can check out more examples [here](https://github.com/maicher/cli_spinnable/tree/master/examples).
