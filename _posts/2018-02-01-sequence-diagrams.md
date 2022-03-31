---
layout: post
comments: true
title: 'Sequence diagrams'
author: Krzysztof Maicher
date: 2018-02-01 13:11:04
categories:
tags: [sequence diagrams, uml diagrams, ruby, software development]
---

What is the most convenient way of communicating a design idea to another developer? In my opinion - drawing it. However, when you are working remotely it's not as easy as taking a marker and walking to a whiteboard.

I was involved in a remote discussion once about redesigning part of an application that the team was working on.
I found myself struggling to explain my design ideas to the other developers.
_"If only I could just draw it!"_ - I thought.
At that moment I was unprepared and I wasn't aware of any tool that would let me draw diagrams as easily as on a whiteboard.

### How developers communicate design ideas?

**Sequence diagrams** are an excellent way to visualize the design ideas. They are described in the Unified Modelling Language (UML) as a group of [Interaction Diagrams](http://wiki.c2.com/?InteractionDiagram).

<blockquote>

They (sequence diagrams) bring clarity to your (developers) thoughts and provide a vehicle to collaborate and communicate with others. Think of them as a lightweight way to acquire an intention about an interaction. Draw them on a whiteboard, alter them as needed, and erase them when they've served their purpose. [1]

</blockquote>

To illustrate what sequence diagrams are, let's define two classes:

<script src="https://gist.github.com/maicher/c950f384ee15a48d44e7b51c1de94c4a.js?file=sequence_diagrams_01.rb"></script>

And a third class that depends on them:

<script src="https://gist.github.com/maicher/c950f384ee15a48d44e7b51c1de94c4a.js?file=sequence_diagrams_02.rb"></script>

Below, is a sequence diagram that illustrates the above code:

{% include image_full_width.html url="sequence.png" description="Sequence diagram" %}

Object C sends the message <code>call</code> to object A (invokes a method <code>call</code> on object A).

Object A responds returning <code>result</code> to object.

Then, object C calls object B passing the obtained result as an argument and so on.

What else can we see besides the interaction between the objects? We can see which methods are being used, so the public interface of each object becomes clear. And, along with that: the boundaries between the objects and their responsibilities.

Thanks to that, looking at a sequence diagram helps to **develop a quality design**.

### How to draw sequence diagrams during remote meetings?

Sequence diagrams, during a design process, usually need to be redrawn and changed. They evolve back-and-forth while a team is brainstorming and trying to find the best design. There are tools that allow for the drawing of sequence diagrams by dragging-and-dropping boxes and drawing arrows. However, I couldn't imagine using them as drawing takes too much time and the listeners would lose their patience.

I was searching for a tool that would give me the possibility to efficiently adjust the diagram in "real time" - on the fly, while elaborating and interacting with others. Take a look:

<div class="video-container"><iframe src="https://www.youtube.com/embed/ZHBBLSA3cFk?rel=0" allowfullscreen="allowfullscreen" width="560" height="315" frameborder="0"></iframe></div>

As you watched the video above, you would have seen me typing code. The diagram was generated on the fly, as I was typing the commands into the editor, making the whole experience pretty interactive. I was using a standard called [PlantUML](http://plantuml.com/sequence-diagram) <a href="#anchor-3">[3]</a> with RubyMine [IDE plugin](https://plugins.jetbrains.com/plugin/7017-plantuml-integration).

### Basic PlantUML syntax for sequence diagrams

First of all, mark the beginning and end of the diagram by keywords <code>@startuml</code> and <code>@enduml</code>.

<script src="https://gist.github.com/maicher/c950f384ee15a48d44e7b51c1de94c4a.js?file=sequence_diagrams_03.puml"></script>

Then draw the diagram putting each step in a separate line and using arrows <code>--&gt;</code>, <code>&lt;--</code> to indicate the direction of the communication.

<script src="https://gist.github.com/maicher/c950f384ee15a48d44e7b51c1de94c4a.js?file=sequence_diagrams_04.puml"></script>

To show the processing use: <code>activate</code> and <code>deactivate</code> keywords.

<script src="https://gist.github.com/maicher/c950f384ee15a48d44e7b51c1de94c4a.js?file=sequence_diagrams_05.puml"></script>

Notes can be added by using <code>note</code> keyword.

<script src="https://gist.github.com/maicher/c950f384ee15a48d44e7b51c1de94c4a.js?file=sequence_diagrams_06.puml"></script>

The whole syntax is described on the PlantUML website.

[1] Sandi Metz, <em>Practical Object-Oriented Design in Ruby</em>, Addison-Wesley, 2013, p.66
