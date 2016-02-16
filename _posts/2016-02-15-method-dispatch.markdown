---
layout: post
section-type: post
title: Method Dispatch
category: design
tags:
- design
---
When writing my earlier post about the Visitor pattern, I came across mention of method dispatch, which encompasses the linked concepts of static dispatch, dynamic dispatch, double/multiple dispatch, and single dispatch.

To understand method dispatch, we should define the terms "message" and "method."

A message has a name and zero or more "arguments," which can be objects, functions, or primitives - depending on the language in question.

A method is a block of code that is triggered in response to a message. The accompanying arguments are available for use within the method. The code of a method can be considered an implementation of the message.

Method dispatch is the means of deciding which method will be triggered by a message. Programming languages can differ significantly in the way they handle method dispatch.

## Different forms of method dispatch

Static dispatch refers to method dispatch that is decided at compile time (as opposed to at runtime). This was an "aha" moment for me, when I realized that this is why static methods are called "static" instead of "class methods" or something else. For example, ```java.util.Collections.sort(list)``` resolves to the ```sort``` method defined in Java's ```Collections``` class. This is decided at runtime because there are possible recipients for the ```sort``` message other than the ```Collections``` class.

Dynamic dispatch refers to method dispatch that occurs at runtime. For example, say we have a ```Bird``` interface that requires a ```sing``` method and is implemented by ```Dove``` and ```Parakeet```. The method to be invoked by calling the ```sing``` method on a ```Bird``` object cannot be decided until runtime. This is because it could be either a ```Dove``` or a ```Parakeet``` object, which each have their own ```call``` method.

Single dispatch and multiple dispatch are forms of dynamic dispatch.

Single dispatch determines which method to invoke based on the message and the recipient. This is a common way of handling method dispatch, such as in C++, Java, JavaScript, and Python.

Multiple dispatch determines which method to invoke based on the message, the recipient, and one or more of the message's arguments. Some languages that natively implement multiple dispatch are Common Lisp, Haskell, and C# (version 4.0 and up).

Take the method call ```foozle.defrimbulate(frimble)``` as an example. In single dispatch, the method would be decided based on the type of ```foozle``` and the message name ```defrimbulate```. In multiple dispatch, the type of both ```foozle``` and ```frimble``` would determine which method to invoke in response to ```defrimbulate```.

## Relevance to the Visitor Pattern

The [Visitor Design Pattern](http://scarvill91.github.io/design/2016/01/19/the-visitor-pattern.html) provides a means of implementing multiple dispatch in languages that don't support it natively.  

The pattern involves passing an object to a method, which calls another method on the passed object and passes itself as an argument to the called method. This double occurrence of single dispatch allows the selection of a method based on both the recipient and the arguments of the original message - implementing a form of double (i.e. multiple) dispatch.

## Note on methods versus functions

While I use the term "method" predominantly in the post, non-object-oriented languages also must decide what to invoke in response to a message. In this case, the appropriate term is "function" rather than "method."

The definition provided for methods in this post also accurately describes functions. Methods are really a specific kind of function - one that is associated with an object and which can operate on data contained within that object.
