---
layout: post
title:  "The Transformation Priority Premise"
date:   2015-09-23
categories:
- design
---
Uncle Bob advances an idea called the Transformation Priority Premise. It is a heuristic for choosing the kind of changes made in order to get tests to pass in the TDD cycle - in other words, a guide for the decisions you make to get from 'red' to 'green' in the process of 'red, green, refactor.'

The postulated benefits of adhering to the TPP include:

- Reducing the amount of time and effort a developer spends getting their tests from failing to passing in each TDD cycle.
- Improving the structure of developed code - in terms of simplicity and ease of change.
- Improving the computational efficiency of developed code - in terms of the algorithms used.

Thus far, the evidence for the above assertions is mostly anecdotal - I think the supporting argument is strongest for the first claim, weaker for the second, and weakest for the third. I'm going to explain the TPP in greater depth and then consider the support for each potential benefit.

# Favoring simpler transformations

A transformation is defined as an atomic change in code that results in a change in the code's behavior. The idea behind the Transformation Priority Principle is that there are many different kinds of transformations you can make every time you need to change code to get a test to pass, and that some transformations have more positive results than others. Therefore, certain kinds of transformations should be given priority over other kinds.

# Types of transformations

Let's look at the types of transformations that Uncle Bob came up with - listed from simplest to most complex. This version of the list is taken from Uncle Bob's [post on TPP](http://blog.8thlight.com/uncle-bob/2013/05/27/TheTransformationPriorityPremise.html) and is supplemented with my own elaborations:

1. ({}–>nil) replacing no code at all with code that represents 'nothing'
2. (nil->constant) replacing nil with some value
3. (constant->constant+) replacing a simple constant with a more complex constant
4. (constant->scalar) replacing a constant with a single-value variable
5. (statement->statements) adding unconditional statements
6. (unconditional->if) adding a conditional, splitting the execution path
7. (scalar->array) replacing a single value with an array
8. (array->container) replacing an array with a more complex datatype, such as a hash or linked list
9. (statement->recursion) adding a recursive call to a statement
10. (if->while) replacing a conditional with a loop
11. (expression->function) replacing an expression with a function
12. (variable->assignment) mutating the value of a variable

Uncle Bob stresses that this ordering may be incorrect, and that there are likely other transformation types that are missing from this list. Indeed, by the time of his videos explaining the TPP on [Clean Coders](https://cleancoders.com/), Uncle Bob's list has changed. In the videos, it reads:

1. Null - same as ({}–>nil)
2. Null to constant - same as (nil->constant)
3. Constant to variable - same as (constant->scalar)
4. Add computation - similar to (statement->statements), but essentialy adding any simple computation like 1 + 1 or 3 % 2.
5. Split flow - same as (unconditional->if)
6. Variable to array - same as (scalar->array)
7. Array to container - same as (array->container)
8. If to while - same as (if->while)
9. Recurse - same as (statement->recursion)
10. Iterate - new! adding an iterative loop
11. Assign - same as (variable->assignment)
12. Add case - new! adding a conditional that splits flow into more than two branches

The takeaway from this is that there is no exhaustive list of transformations, and their relative complexity is not set in stone. Notice how (expression->function) disappears from the first list, and how the relative position of (if->while) and (statement->recursion) is swapped in the two lists. I'm pretty unclear on what constitutes 'replacing a simple constant with a more complex constant' in (constant->constant+) (maybe that's why it is also conspicuously absent in the second list). In the second list, I'm not sure there's a solid distinction between the transformation of 'If to while' versus 'Iterate' - 'Iterate' just seems like the more general case, in which case 'If to while' should probably be subsumed by that one.

Uncle Bob's core insight, however, is a valuable one. We *should* be thinking about the **kinds** of transformations we're making and about their **relative complexity** when compared to each other. This enables us to be deliberate about the kinds of transformations we make, and to be mindful of any benefits or costs that result from using one kind over another.

# The hypothesized benefits

Uncle Bob's first argument is that **abiding by the TPP "will prevent impasses, or long outages in the red/green/refactor cycle."**([1](http://blog.8thlight.com/uncle-bob/2013/05/27/TheTransformationPriorityPremise.html)) If we accept that simpler changes to code are faster and easier to implement correctly, then Uncle Bob's assertion follows readily from the fact that the TPP dictates that less complex transformations are of higher priority than more complex ones. This can also be empirically tested pretty easily - just compare the average time spent between red and green when abiding by the TPP versus when not. Better yet, compare the average time between abiding by the TPP versus abiding by the inverse of the TPP.

In the conclusion of his [two-parter episode on the TPP](https://cleancoders.com/episode/clean-code-episode-24-p1/show on Clean Coders, Uncle Bob also asserts that **following the TPP "almost certainly help[s] us to write better systems."** If you accept that prioritizing simpler transformations over more complex ones (when possible) will produce code that is simple, then it follows that the TPP can help us produce simpler systems. In my opinion, this premise is a little more tenuous. As a counterexample, consider the fact that any computation boils down to a a long series of relatively simple mathematical computations on numeric values at the assembly level. Replicating this in the code we write would utilize only the highest priority transformations of the TPP, but that certainly wouldn't result in the most simple, transparent, or flexible system.

The judgement required to apply the TPP is not encapsulated within the concept - you have to rely on additional heuristics to determine what kinds of transformations are up for consideration any given point. I think Uncle Bob's conjecture that the TPP can guide us to simpler, better systems - but only when it is utilized in conjunction with other principles. For example, the applying the [DRY principle](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) would encourage the use of some of the more abstract, lower-priority transformations and prevent us from going down a very low-level, repetitous path like in the above counterexample.

Based on a scenario in which abiding by the TPP results in the implementation of the merge sort algorithm and where neglecting the TPP results in a bubble sort algorithm, Uncle Bob also poses the question of whether **the TPP reliably causes better algoritms to be implemented.**([2](https://blog.8thlight.com/uncle-bob/2013/05/27/TransformationPriorityAndSorting.html)) To be honest, I'm not sure one way or another. This would be hard to test emperically - with foreknowledge of any given algorithm and its relative efficiency practically guaranteed to subtly influence our tests and the changes we consider making to our code. That said, I'm not too concerned about it. In the case that computational efficiency matters, we will most likely have a preferred algorithm in mind anyway.