---
layout: post
title:  "Three ways that developers practice"
date:   2015-09-21
categories: practice education katas
---

I've been asked to begin learning Swift this week - and the way I've been asked to do that is to use Swift to implement and demonstrate the [prime factors kata](http://butunclebob.com/ArticleS.UncleBob.ThePrimeFactorsKata) to my mentors. This, and reading about what Uncle Bob has to say about practicing and katas in *Clean Coder*, leads me to write this post examining some of the established ways in which software developers practice their craft.

# Code Katas

Code katas, as originally conceived by Dave Thomas, really don't describe much more than opportunities for deliberate, focused practice.[[1](http://codekata.com/)] The concept has evolved, in some circles at least, to more closely mirror the meaning of the Japanese term from which it takes its name. Wikipedia defines the Japanese term:

> Kata (literally: "form"), are detailed choreographed patterns of movements practised either solo or in pairs...By practicing in a repetitive manner the learner develops the ability to execute those techniques and movements in a natural, reflex-like manner. Systematic practice does not mean permanently rigid. The goal is to internalize the movements and techniques of a kata so they can be executed and adapted under different circumstances, without thought or hesitation.

Modeled after this meaning, code katas become a very particular form of programming practice - one centered on memorization, repetition, and automaticity. You take a small problem - generating the prime factors of a given number, or tracking the score of a bowling game - and solve it. Or you take someone else's solution and implement it yourself. Then you solve it again and again - a little better and a little faster each time. There is no stopping-point - the kata simply becomes part of your repertoire, more and more deeply imbedded in your muscle-memory and autonomous brain-processes.

There are a number of arguments in favor of this form of practice. In my case, it's an opportunity to become familiar with a new language by hammering its basic forms and syntax into my brain until they become automatic. Ditto for learning the ins and outs of using xcode. I'll also quickly internalize the algorithm for generating prime factors; when I want to try another language in the future, I can implement the algorithm in that language and focus exclusively on understanding the novel style and syntax, rather than figuring out the solution.

Uncle Bob also suggests that implementing someone else's solution (including every addition and deletion in the process of arriving at the final product) can be a tool for understanding their approach to software design. As he puts it:

> Perhaps the best way to acquire "Design Sense" is to find someone who has it, put your fingers on top of theirs, put your eyeballs right behind theirs, and follow along as they design something. Learning a kata may be one way of accomplishing this.[[2](http://www.butunclebob.com/ArticleS.UncleBob.TheBowlingGameKata)]

# Personal Projects

Personal projects are a great way to practice software development - you don't have the pressures associated with developing a commercial product, you can collaborate with and learn from others, and it provides the opportunity to use technologies that you wouldn't otherwise work with on a regular basis. The only potential downsides that occur to me are that helpful feedback may be hard to come by if working on a relatively unknown app, it may be easier to lose focus on a specific area of improvement when working on a large project, and the pressures of creating a complete and working project may still distract from the goal of effective practice. That said, being able to show others a tangible something that demonstrates what you have learned can be a powerful motivating force.

# Code Retreats

At a code retreat, a number of developers spend a significant period of time (usually a whole day) to tackle a coding problem in a number of different ways. Programming in pairs, the developers might develop 4-6 different solutions to the same problem in 1-2 hour blocks of time. Each iteration is an opportunity to change pairings and to attempt solving the problem in a different way - perhaps using a more functional style, keeping function definitions to a minimum size, etc.[[3](http://coderetreat.org/about)] Thus, some of the primary aims of a code retreat are to develop skills within many complimentary avenues of potential improvement, on breaking out of habitual patterns of software development, and on the spontaneous exchange of knowledge between partners.

