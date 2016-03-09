---
layout: post
section-type: post
title:  Pairing Tour Day 7
category: learning
tags:
- learning
---
I paired with Byron today - working in Ruby for the first time since my application exercise for the apprenticeship. We spent the day improving part of a project that handled boolean searches of records in an application.

It was a very big application. It was only partially tested.

As a result, it was very daunting to contemplate changing it. And I wanted to change a lot of things.

There was plenty of fine code. But there was also redundant code, there was indecipherably convoluted code, and there was poorly designed and organized code.

For example, a widely-used object possessed several boolean attributes that served as flags. These flags were poorly named, so each one was wrapped in a private method in a client object that made the flags' meanings clear.

These private methods were used in conditionals inside another private method, each of which determined whether to add a constant to a collection that indicated... something, I'm unclear what. But I'm sure that what I described is a far more convoluted path to that something than was necessary.

It would be nice if the widely-used object encapsulated this logic, and was able to share that collection rather than those boolean flags.

But a refactor to do that would be risky - given the spotty test coverage - and would probably take all day. And that's probably the most valuable lesson from today!

Code gets crufty, you have to pick and choose the ways you try to improve it, and when you do, the progress is agonizingly incremental. I'd gotten that impression from reading [Working Effectively with Legacy Code](http://www.amazon.com/Working-Effectively-Legacy-Michael-Feathers/dp/0131177052), and so it was instructive to see that manifest in practice.

That said, we got some things done. In the morning, Byron and I knocked out a quick refactor of a class that he'd written the previous week - changing it from a class to a module per feedback on his PR. That went smoothly enough.

In the afternoon, we worked on adding 'disclaimers' to the boolean search part of the project - though I'm still not entirely sure how it all fit together. Disclaimers were part of the search results, or were searchable themselves, or something like that.

The new code went relatively smoothly, but Byron and I (and now also an employee of the client - we were triple-pairing, tripling?) inevitably returned to the very same object I described to you above and remained mired in an attempted refactor of it and associated elements for the remainder of the day.

I struggled to understand the overall context, business needs, current functionality of the app, and intended design. I'll admit I had a very hard time doing this, and most of my input related to relatively small tactical decisions - such as improvements in which Ruby methods were used or how a block of code was structured.

I also hope that I put my vast ignorance to as much use as possible, asking questions which Byron occasionally did not have the answer to, Googling it, and sharing the knowledge I found.

This is how I found out how ```git checkout -b``` differs from ```git checkout -B``` (the capitalized flag will 'reset' the specified branch if it already exists), and that prefixing things with Ruby's scope resolution operator ```::``` is scoping that thing to the top-level, or 'global', scope.
