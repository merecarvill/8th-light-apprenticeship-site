---
layout: post
section-type: post
title:  "Anonymous functions in Ruby #1 - Blocks, Procs, & Lambdas"
category: ruby
tags:
- ruby
---
Anonymous functions and anonymous function-like behavior are quite pervasive in Ruby, and I really wasn't particularly aware of it all until I recently took the time to understand it a bit better. I was prompted to dig deeper in an attempt to write a generic [Negamax](https://en.wikipedia.org/wiki/Negamax) algorithm that could be passed context-specific functionality that would allow it to be applied within any game. I hope that sharing what I've learned might inspire some new ideas for tackling similar problems, or at least provide some interesting brain-candy.

The term 'anonymous function' is relatively easy to unpack at the semantic level. It describes a function - a bit of code with input, output, and which 'does' something in-between - and it is anonymous - you can't reference it by name, you can only use the function if you've just defined it or been given it by something else. But what does that mean concretely? This may be easiest to explain by examining how anonymous functions are implemented in Ruby.

# Procs and Lambdas

Procs and Lambdas are the true 'anonymous function' in Ruby. They look like this:

<pre style="text-align: left">
p = Proc.new { |greeting| puts "#{greeting}, world!" }
p.call("Hello")
# > Hello, world!
</pre>

<pre style="text-align: left">
l = lambda { |greeting| puts "#{greeting}, world!" }
l.call("Hello")
# > Hello, world!
</pre>

You may also come across the 'stabby lambda' syntax, which allows lambdas to be defined like so:

<pre style="text-align: left">
->(greeting) { puts "#{greeting}, world!" }
</pre>

As you can see, procs and lambdas are defined in a very similar manner and both define a function complete with specified parameters and a return value. They encapsulate this function as an object in Ruby - each can be stored in a variable and passed around, and their function can be applied using the ```:call``` method. Procs and lambdas differ in subtle ways that I think I'll save for a later post. Before finishing this post, I'd like to first bring Ruby's blocks into the comparison.

# Blocks

A Ruby block is one of the rare elements of Ruby that is not an object - unlike procs and lambdas - and isn't technically an anonymous function. However, blocks fill a very similar niche and were what I was accustomed to using in Ruby until deliberately familiarizing myself with procs and lambdas.

Blocks may be easiest thought of simply as 'blocks of code.' In that sense, blocks define a set of operations, but don't come with any of the machinery that allow them to be passed around or used as functions in their own right. You may recognize them as the special argument in brackets that you pass to Ruby methods like ```:map```, ```:select```, and ```:reduce```. For example:

<pre style="text-align: left">
[1, 2, 3].map { |num| num*2 }
# => [2, 4, 6]

[1, 2, 3].select { |num| num.odd? }
# => [1, 3]

[1, 2, 3].reduce { |sum, num| sum + num }
# => 6
</pre>

The blocks are the elements delineated by curly braces, and each specifies an operation to be performed (for each element in a datastructure, in the above examples). To reiterate, each block is only valid as an argument, and is not an object in and of itself. In addition, any given method cannot accept more than a single block among its arguments. You may have noticed that, in the earlier example of procs and lambdas, both accept a block at intialization that defines the operations to occur when their ```:call``` method is invoked.