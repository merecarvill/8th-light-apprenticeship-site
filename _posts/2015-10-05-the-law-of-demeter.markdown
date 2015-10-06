---
layout: post
title: The Law of Demeter
date: 2015-10-05
categories:
- design
---
The Law of Demeter (LoD) is another heuristic intended to promote good object-oriented design.

# Definition

The wikipedia article describes the central idea succinctly:

> The fundamental notion is that a given object should assume as little as possible about the structure or properties of anything else (including its subcomponents), in accordance with the principle of "information hiding".

How precisely to do this has been described in a number of ways. These range from the pithy "Don't talk to strangers," to the exhaustive formulation by Karl J. Lieberherr, Ian M. Holland, and Arthur J. Riel in a 1988 paper titled *[Object-Oriented Programming: An Objective Sense of Style](http://www.ccs.neu.edu/research/demeter/papers/law-of-demeter/oopsla88-law-of-demeter.pdf)*:

> For all classes C, and for all methods M attached to C, all objects to which M sends a message must be:
>
> - M's argument objects, including the self object or...
> - The instance variable objects of C.
>
> (Objects created by M, or by functions or methods which M calls, and objects in global variables are considered as arguments of M.)

In plain speech, the LoD says that any method you write is allowed to call another only if it:
- belongs to the same class/object
- belongs to an object stored in an instance variable of the calling object
- belongs to an object passed in as an argument
- belongs to an object created within the method
- belongs to an object stored in a global variable (though I bet most would come down against this last one nowadays)

# Another way of looking at the LoD

Given all of the conditions defined in the previous section, I find it easier to think of the LoD in terms of what it suggests should be avoided. Namely, we shouldn't call methods of an object accessed by calling another method. People are sometimes overeager in their interpetation of this, saying that any statement with "multiple dots" - such as ```object.foo.bar``` - is a violation of the LoD. That *could* be a violation, but not necessarily - the important thing is whether ```object.foo``` returns an object that is the same class as ```object``` itself. If this is the case, such as we might see in string manipulations like ```some_string.strip.downcase```, all is well. A violation, on the otherhand, might look like ```some_string.split("").first``` or like this:

{% highlight ruby %}
class Foo
  def foo_method
    Bar.new
  end
end

class Bar
  def bar_method
    "you better not have called this method through Foo!"
  end
end

foo = Foo.new
# This is a LoD violation:
foo.foo_method.bar_method
{% endhighlight %}

In both cases, the object returned midway through the sequence of method calls is not the same class as the original object and is therefore a violation of the LoD.

# Potential benefits

Following the Law of Demeter is intended to facilitate encapsulation of information in code. It prevents elements in a system from depending on the structural details of other elements. As the result of such a dependency, ```Foo``` objects could cease to function correctly as the result of changing the implementation of ```Bar``` and/or ```bar_method``` in the previous example. In more pathological scenarios, the LoD can tie in with the principle of "[Tell, don't ask](https://pragprog.com/articles/tell-dont-ask)" - when a LoD violation is a sign that your code's logic is misplaced and should be moved from the calling object to the object being called.

The LoD also generally makes code easier to follow. Even the trivial example of ```some_string.split("").first``` might be easier understood if it was divided into two statements: ```some_characters = some_string.split("")``` followed by ```some_characters.first```.

