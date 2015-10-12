---
layout: post
section-type: post
title:  "Keyword arguments in Ruby"
category: ruby
tags:
- ruby
---
I made the rather pleasant discovery that keyword arguments are supported in Ruby 2.0+ the other day. It's funny that I should find this out now, considering that I'm just starting to learn Swift, which is the first language I've been exposed to that makes prevalent use of keyword arguments. Until now, I'd drifted towards using parameter hashes for passing arguments in Ruby, having osmosed the practice from POODR and random code examples without having thought too hard about it.

# Ordered arguments vs. keyword arguments

Ordered arguments (also known as positional arguments) are associated with correct identifiers as a result of the order in which they were passed to a method:

<pre style="text-align: left">
def ordered_args(one, two, three)
  puts "one: #{one}, two: #{two}, three: #{three}"
end
ordered_args(1, 2, 3)
# > one: 1, two: 2, three: 3
</pre>

Keyword arguments, on the other hand, are values explicitly associated with a keyword that serves to identify them and can therefore be passed to a method in any order:

<pre style="text-align: left">
def keyword_args(one:, two:, three:)
  puts "one: #{one}, two: #{two}, three: #{three}"
end
ordered_args(one: 1, three: 3, two: 2)
# > one: 1, two: 2, three: 3
</pre>

# Advantages of keyword arguments

- Clarity: you need look no further than a method signature in order to learn the names of arguments, rather than having to search the body of the method.
- Safety: it's impossible to mistakenly pass arguments in the wrong order, causing runtime errors - or worse, deceptively appearing to work as planned and instead returning and propagating an incorrect value.
- Flexibility: you'll no longer have to find and correct every location where a method is used if the order of arguments happens to change - in fact, you're free to use different orderings in different contexts if it benefits the clarity of your code

# Default parameters demo

If you're new to using keyword arguments in Ruby, I thought I'd add a brief demo of how to define default parameter values using the keyword syntax:

<pre style="text-align: left">
def default_parameter_test(parameter: "default")
  puts parameter
end
default_parameter_test
# > default
default_parameter_test("foo")
# => ArgumentError: wrong number of arguments (1 for 0)
default_parameter_test(parameter: "foo")
# > foo
</pre>