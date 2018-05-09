---
layout: post
comments: true
title: 'How to keep rspecs DRY'
author: Krzysztof Maicher
date: 2017-06-28 16:00:00
categories:
tags: [ruby, rspec, dry]
---

### Intro

I want to share my thoughts about a specific, __low-level duplication problem__, that I see often in _rspec_ tests.

### Why?

Tests are software, so design concepts apply to them as well.
One of the key things is to keep them [DRY](http://wiki.c2.com/?DontRepeatYourself).

In a words of Sandi Metz from _Practical Object-Oriented Design in Ruby_:

> _Removing duplication from testing lowers the cost of changing them in reaction to application changes(..)_[[1]](#1)

### Use case

Take a look at this Ruby class.

```ruby
class Foo
  def initialize(dependency:)
    @dependency = dependency
  end
  
  def call(an_argument)
    #...
  end
end
```

A class with one dependency that responds to one method which takes one argument.

For simplicity, lets assume that `#call` is a [query method](https://martinfowler.com/bliki/CommandQuerySeparation.html),
which can be tested by asserting a return value.

### SPECS

What I've observed, developers tend to write specs structured more or less like:

```ruby
describe Foo do
  let(:a_dependency) { .. }
  let(:other_dependency) { .. }
  
  describe '#call' do
    context 'when initilaized with a_dependency' do
      context 'when something' do
        let(:argument) { .. }
        subject { Foo.new(dependency: a_dependency) }
        it { expect(subject.call(argument)).to eq(123) }
      end

      context 'when not something' do
        let(:argument) { .. }
        subject { Foo.new(dependency: a_dependency) }
        it { expect(subject.call(argument)).to eq(456) }
      end
    end

    context 'when initialized with other_dependency' do
      context 'when something' do
        let(:argument) { .. }
        subject { Foo.new(dependency: other_dependency) }
        it { expect(subject.call(argument)).to eq(234) }
      end

      context 'when not something' do
        let(:argument) { .. }
        subject { Foo.new(dependency: other_dependency) }
        it { expect(subject.call(argument)).to eq(567) }
      end
    end
  end
end
```

In above specs, different contexts are covered and subjects are described.
I believe that by looking at it you can tell what is going on there.

However, there are some __repetitions__ which violate DRY principle:

- Tested class name `Foo` is used few times (in lines: 1, 9, 15, 23, 29).
- Knowledge about _how to instantiate_ tested class is duplicated in each example (lines: 9, 15, 23, 29).
- Knowledge about _how to call tested method_ is duplicated as well (lines: 10, 16, 24, 30).

Therefore, if one of the followings change:
- class name, 
- way of initializing (more/less injected dependencies),
- method name,
- arguments number.

..if one of above change in code, then 4-5 specs lines need to be adjusted keep the tests up to date.

It's not _that_ problematic, but still, improving it makes sense.

### Refactor #step 1 - move _lets_ into their _contexts_

Because declaring them on the top makes an impression that two dependencies need's to be initialized in order to run the specs.
Which is not true - only one dependency is needed. There are two of them, because of two _contexts_.
So moving them into their _contexts_ makes it easier to follow.

_Contexts_ descriptions is making it clear how they vary between each other and the fact
that they have the same names makes it easier to understand that they represent the same being (lines: 4, 20)


```ruby
describe Foo do
  describe '#call' do
    context 'when initilaized with a dependency' do
      let(:dependency) { .. }
      
      context 'when something' do
        let(:argument) { .. }
        subject { Foo.new(dependency: dependency) }
        it { ... }
      end

      context 'when not something' do
        let(:argument) { .. }
        subject { Foo.new(dependency: dependency) }
        it { ... }
      end
    end

    context 'when initialized with other dependency' do
      let(:dependency) { ... }
      
      context 'when something' do
        let(:argument) { .. }
        subject { Foo.new(dependency: dependency) }
        it { ... }
      end
      
      ...
    end
  end
end
```

### Refactor #step 2 - keep knowledge how to initialize tested class in one place

Because this knowledge is common for all examples.

In different _contexts_, different dependencies are used to instantiate tested class.
These dependencies can be declared later (in line 6), in their contexts, after _subject_ declaration (line 3).

It works because of _subject_'s lazy evaluation - code in _subject_'s block is evaluated in line 7, not in line 3.

```ruby
describe Foo do
  describe '#call' do
    subject { Foo.new(dependency: dependency) }

    context 'when initilaized with a dependency' do
      let(:dependency) { .. } 
      it { expect(subject.call(argument)).to eq(..) }
    end

    # ...
  end
end
```

If we wanted to test more methods, we would have to create more _describe_ blocks.
In this case I would suggest extracting the knowledge about how to initialize tested class to a _let_ block at the top.
After that - reuse it in _subject_ declarations.

```ruby
describe Foo do
  let(:foo) { Foo.new(dependency: dependency) }

  describe '#a_method' do
    subject { foo.a_method(argument) }
    # ...
  end
  
  describe '#other_method' do
    subject { foo.other_method(argument) }
    # ...
  end
end
``` 
### Refactor #step 3 - keep knowledge how to call tested method in one place

For the same reasons as in previous step.

Instead of duplicating `subject.call(argument)` in each expectation, it could be moved into the
_subject_, because it's the same in each case (line 5 in below listing).


### Final version

- Tested class name is used once (line: 1 - [described_class](https://relishapp.com/rspec/rspec-core/docs/metadata/described-class)).
- Knowledge _how to instantiate_ tested class is kept in one place (line: 2).
- Knowledge _how to call tested method_ is kept in one place, following it's _describe_ block (line: 5).
- _let_ blocks are declared within their _contexts_ (lines: `8 and 22`, `11 and 16`, `25 and 30`).

As a result of eliminating these duplications, specs became also more readable.
When looking at the nesting levels, we can see a kind of __descending from general to detailed__ structure.

__The general things are declared higher, closer to the top and the context-related details are nested deeper.__   

```ruby
describe Foo do
  let(:foo) { described_class.new(depencency: dependency) }
  
  describe '#call' do
    subject { foo.call(argument) }

    context 'when initilaized with a dependency' do
      let(:dependency) { .. } 

      context 'when something' do
        let(:argument) { .. }
        it { expect(subject).to eq(123) }
      end

      context 'when not something' do
        let(:argument) { .. }
        it { expect(subject).to eq(456) }
      end
    end

    context 'when initialized with other dependency' do
      let(:dependency) { .. }

      context 'when something' do
        let(:argument) { .. }
        it { expect(subject).to eq(234) }
      end

      context 'when not something' do
        let(:argument) { .. }
        it { expect(subject).to eq(567) }
      end
    end
  end
end
```

Practice writing DRY specs. This habit pays of.

#### More reading

- <a name='1'>[1]</a> Sandi Metz, _Practical Object-Oriented Design in Ruby_, Addison-Wesley, 2013, p.195
- [The Magic Tricks of Testing by Sandi Metz (video)](https://youtu.be/URSWYvyc42M)
- [Rspec Style Guide](https://github.com/reachlocal/rspec-style-guide)
- [www.betterspecs.org](http://www.betterspecs.org)
