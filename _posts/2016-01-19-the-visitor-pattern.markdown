---
layout: post
section-type: post
title: The Visitor Pattern
category: design
tags:
- design
---
The Visitor design pattern presents a way to separate an algorithm from the objects on which it operates. The [Gang of Four](https://en.wikipedia.org/wiki/Design_Patterns) outlined the Visitor pattern like so:

> "Represent an operation to be performed on elements of an object structure. Visitor lets you define a new operation without changing the classes of the elements on which it operates."

In this way, the Visitor pattern can useful to extract code that violates the [Single Responsibility Principle](http://scarvill91.github.io/design/2015/10/21/the-single-responsibility-principle.html), and to abide by the [Open/Closed Principle](http://scarvill91.github.io/design/2015/10/22/the-open-closed-principle.html) by being able to define new behavior without the need to modify existing code.

I've come across a number of example implementations of the Visitor pattern, including those situated in such obligatory domains as calculating the pay of various ```Employee``` types, drawing various ```Shapes``` in a collection, and printing the appropriate animal sound of different ```Animal```s to the command line. Each example served to help me better understand what the Visitor pattern looks like. However, I still struggled to grasp what kind of real-world situations are particularly suited to the application of the Visitor pattern and the concrete gains it offers.

That said, definitely consider checking out the example implementation of the Visitor pattern on [wikipedia](https://en.wikipedia.org/wiki/Visitor_pattern#Java_example).

My limited experience has yet to offer an ideal situation for applying the Visitor pattern, but I think the [Gilded Rose Kata](http://iamnotmyself.com/2011/02/13/refactor-this-the-gilded-rose-kata/) is a good candidate. The kata presents the problem of refactoring a fantasy-world shop's legacy inventory application. I worked on a version in Clojure whose [function to update inventory items](https://github.com/scarvill91/gilded-rose-clojure/blob/master/src/gilded_rose/core.clj) was initially a single tangled mass of conditionals that branched a dozen times based on the type of item.

This kind of branching logic based on checking type is one way of defining behavior for objects of different types. This kind of code is no fun at all to try to understand or modify. Code duplication is usually rampant across the different branches, additional control flow in a branch means that you have to pop two or more contexts onto your mental stack to be able to understand what's going on, and defining new behavior means modifying the exisiting code, which risks breaking everything.

As I refactored the code, I found myself wanting the kind of polymorphic behavior that is enabled by the Visitor pattern: to be able to iterate through the collection of inventory items, selecting the appropriate algorithm for updating each item based on its type, yet without having to define that behavior as part of the items themselves. Because Clojure supports [multiple dispatch](https://en.wikipedia.org/wiki/Multiple_dispatch), I was able to achieve this kind of behavior pretty easily without having to use the Visitor pattern: you can the final result [here](https://github.com/scarvill91/gilded-rose-clojure/tree/solution/src/gilded_rose).

If I had been using a statically-typed, object-oriented language and I didn't want the update behavior to be defined as part of each type of inventory item, I would have encapsulated that behavior in a separate class using the Visitor pattern. This would be a good choice if there was duplication across update behavior (which there was) or if there were multiple, distinct sets of behavior for updating items. If the latter case were true, a good design might define a type of visitor for each set of update behavior.

I should note that the Visitor pattern doesn't seem to have a suitable application in dynamically-typed languages. Defining differing behavior for different types of objects in a Visitor object requires switching based on type by overloading the ```visit``` method. To achieve something similar in a dynamically-typed language, you'd be replicating the kind of undesirable branching logic based on type checking that I described earlier.
