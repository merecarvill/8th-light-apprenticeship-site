---
layout: post
title:  "Anonymous functions in Ruby #2 - Differences, Details, & Background"
date:   2015-09-17
categories:
- ruby
- language features
---
This post continues my exploration of anonymous functions in Ruby.

# Differences - The subtleties of procs vs. lambdas

To begin, both are instances of the Proc class. To demonstrate, you should see something like this if you inspect them in irb:

{% highlight ruby %}
Proc.new {}
# => #<Proc:0x007fb5e9192c50@(irb):1>
lambda {}
# => #<Proc:0x007fb5e91ae388@(irb):2 (lambda)>
{% endhighlight %}

As you can see, however, a lambda is a special 'flavor' of proc. How that works under the hood, I have no clue. The difference manifests in a couple significant ways, though:

- A lambda will complain if called with the wrong number of arguments, whereas a proc will do the best it can with what it's given. If given too few arguments, a proc will substitute ```nil``` for each missing argument. If called with too many arguments, a proc will just ignore the extras. To demonstrate:

{% highlight ruby %}
p = Proc.new { |x, y| [x.nil?, y.nil?] }
p.call
# => [true, true]
p.call(:foo)
# => [false, true]
p.call(:foo, :bar)
# => [false, false]
p.call(:foo, :bar, :baz)
# => [false, false]

l = lambda { |x, y| [x.nil?, y.nil?] }
l.call
# => ArgumentError: wrong number of arguments (0 for 2)
l.call(:foo)
# => # => ArgumentError: wrong number of arguments (1 for 2)
l.call(:foo, :bar)
# => [false, false]
l.call(:foo, :bar, :baz)
# => # => ArgumentError: wrong number of arguments (3 for 2)
{% endhighlight %}

- A lambda returns control to the method from which it was called, whereas when a proc returns it triggers a return from the enclosing method as well. Here is an example

{% highlight ruby %}
def proc_test
  proc = Proc.new { return }
  proc.call
  puts "This should not print"
end
def lambda_test
  l = lambda { return }
  l.call
  puts "This should print"
end

proc_test # does nothing
lambda_test
# > This should print
{% endhighlight %}

That last example was based on an example from [this post](http://awaxman11.github.io/blog/2013/08/05/what-is-the-difference-between-a-block/) by Adam Waxman, who also offers this helpful - and terse - list of some of the points about blocks, procs, and lambdas covered in this post and my previous post:

- Procs are objects, blocks are not
- At most one block can appear in an argument list
- Lambdas check the number of arguments, while procs do not
- Lambdas and procs treat the ‘return’ keyword differently

# Details - WTF is a closure?

Understanding that procs and lambdas are closures, and that blocks are very like closures, is very important to understanding an important aspect of their behavior. So WTF is a closure? My attempt at a pithy definition is that a closure is a function that retains the referential nature of variables used in its definition. You can contrast this to simply evaluating variables at the time the function is defined and using those values in place of the variable from then on.

In this sense, a closure is a function that remains bound to the environment in which it was originally defined, allowing changes in that environment to be reflected in changes in behavior of the function. Enough talk though. Here is an example that hopefully demonstrates the nature of closures:

{% highlight ruby %}
counter = 0
l = lambda { counter }

def closure_test
  counter += 1
  l.call
end

closure_test
# => 1
closure_test
# => 2
closure_test
# => 3
{% endhighlight %}

The thing to note in the above example is that the lambda preserves the reference to ```counter``` despite the variable having been defined outside of the scope of the lambda's definition. If counter were evaluated at the time of definition, the lambda would return 0 (the original value of ```counter```) every time it was called. Even if the lambda and ```counter``` had been defined far removed from the context in which ```closure_test``` was defined, you would see the exact same behavior. This can lead to some very tricky side-effects, wherein you're modifying variables from the defining context of a proc or lambda and it causes their behavior to change in unanticipated ways.

A last thing to note before moving on: despite the fact that the example given featured a lambda, procs and blocks share this same behavior. Each maintains the reference to variables in the context in which they were defined.

# Background - Lambda calculus

Anonymous functions have their theoretical basis in lambda calculus, which was invented by Alonzo Church in 1936. As you might guess, this is the reason why the keyword ```lambda``` is associated with anonymous functions in Ruby (and many other languages as well). I think a quote from wikipedia offers a great defintion of lambda calculus:

> **Lambda calculus** (also written as λ-calculus) is a formal system in mathematical logic for expressing computation based on function abstraction and application using variable binding and substitution.

For a more formal defintion, see this excerpt from a Cornell CS class resource:

> **A λ-calculus term is:**
>
> - a variable x∈Var, where Var is a countably infinite set of variables;
> - an application, a function e0 applied to an argument e1, usually written e0 e1 or e0(e1); or
> - a lambda abstraction, an expression λx.e representing a function with input parameter x and body e. Where a mathematician would write x ↦ x2, or an SML programmer would write fn x => x*x, in the λ-calculus we write λx.x2.
>
> **In BNF notation:** e ::= x \| λx.e \| e0 e1

I encourage you to read the [wikipedia article](https://en.wikipedia.org/wiki/Lambda_calculus) or the [Cornell resource](http://www.cs.cornell.edu/courses/cs312/2008sp/recitations/rec26.html) in full if you're interested in learning more.