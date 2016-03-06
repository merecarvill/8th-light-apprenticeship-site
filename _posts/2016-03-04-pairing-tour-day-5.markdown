---
layout: post
section-type: post
title:  Pairing Tour Day 5
category: learning
tags:
- learning
---
My mentor Aaron and I paired today. Kudos to him, since he accepted with only 24-hours notice, since I was originally scheduled to pair with someone else who cancelled on me.

It was a Friday, so we spent the morning working on client work and the afternoon (Waza) working on a personal project of Aaron's.

In the morning, Aaron was finishing the setup of a new project using TypeScript and React. Specifically, he was finishing arranging the project's tasks in Gulp to his liking. I'm not sure what the default behavior is in Gulp - if there is one - but Aaron arranged the tasks in composable elements called profiles and targets.

The choice of targets allowed you to, for example, run the tests against just the compiled source files, or the production-ready files that had additionally undergone concatenation, minification, etc.

I was only able to offer pretty basic input - there wasn't an easy way for Aaron to unpack all the context necessary for me to understand his intended design for the project workflow.

In the afternoon, we worked on porting Aaron's Clojure test framework [Coconut](https://github.com/aaronlahey/coconut) to work with ClojureScript. After getting it to work with Clojurescript, he hopes to reconcile the ClojureScript- and Clojure-specific aspects of the project until the same code works for both languages.

As it is though, there's a number of things that have to change to get it to work with ClojureScript. Obviously all dependencies on Java libraries have to be changed.

Coconut is pretty cool. It uses RSpec-style 'expect' syntax, which I'm most familiar with. The project is also entirely tested using itself - a situation that I've always thought was a fascinating challenge.

I think energy levels were pretty low for both of us that afternoon, though. We didn't make much progress.

The main thing we attempted to do was to get 'let'-style declarations working. These bind a symbol to an expression that is available within a scoped context and is lazily evaluated. The resulting value is cached across multiple calls in the same example but not across examples.

I really didn't follow most of it, but it was a challenge to get it working. The level of metaprogramming required is beyond me at this point. There were a couple of times that Aaron used the Clojure backtick, tilde, and single-quote to modify a single element in the code.

And, in the end, we weren't able to get it working. I ended up with a slightly better grasp of Clojure metaprogramming though, and hopefully Aaron came out of it closer to arriving at a solution.
