---
layout: post
section-type: post
title: The Open/Closed Principle
category: design
tags:
- SOLID principles
- design
---
The Open/Closed Principle (OCP) is another of the SOLID principles of good object-oriented design. I've written perviously about other SOLID principles: the [Dependency Inversion Principle](http://scarvill91.github.io/design/2015/10/05/the-dependency-inversion-principle.html) and the [Single Responsibility Principle](http://scarvill91.github.io/solid/2015/10/21/the-single-responsibility-principle.html). The idea behind the OCP is credited to Bertrand Meyer. The commonly cited formulation of the OCP is

> "Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification."([1](http://www.objectmentor.com/resources/articles/ocp.pdf))

I'm sure the above formulation was a lot more transparent in the time and context in which it was initially conceived, but it did absolutely nothing to help my understanding of the principle when I first heard it. The concepts of open vs. closed and extension vs. modification all need further unpacking, and we're basically no better off than when we knew nothing but the name of the principle. If I had to raise a single complaint about the SOLID principles in general, it is that I find the historical particulars and the attemts at abstract formalization of the principles to be an impediment to their comprehension and effective use. I'd love to see an attempt at distilling them with the highest priority given to using the simplest, clearest, and most concrete language possible.

In light of that, I think the OCP can be simply and concisely described as "design code that can be changed by writing new code, rather than modifying existing code." If you find that you can add or change functionality in a piece of software solely by writing new code, then that software is abiding by the OCP - at least for that kind of change.

From this explanation, there arise a few further points to make. For one, the added effort and complexity that comes from applying the OCP is only worthwhile for code that is subject to change. From an evolutionary design perspective, this often means waiting to apply the OCP until you actually have to go back and change or add something in your code. In the process of making that first change, you redesign the code to conform to the OCP so that further changes will be easier. I think it was Uncle Bob who said something along the lines of "we take the first bullet, and then make sure we are protected from any more bullets coming from that particular gun."([2](http://www.amazon.com/Software-Development-Principles-Patterns-Practices/dp/0135974445))

There are a number of ways to apply the OCP. It can often be done by creating and conforming to an interface, whether explicit or implicit. For example, I wrote a ```HumanPlayer``` class and a ```ComputerPlayer``` class in a Ruby implementation of tic tac toe that conformed to an implicit interface of a single public ```move``` method. In the game loop, I was able to get a move from the current player without needing to know which class of player I was dealing with. Adding a new kind of player - such as an 'easy difficulty' computer player that selects random available moves - would simply require me to write a new player class with a public ```move``` method.

My implementation of [Conway's game of life](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life) in Swift that I discussed in my post on the [Single Responsibility Principle](http://scarvill91.github.io/solid/2015/10/21/the-single-responsibility-principle.html) is an example to which the OCP could be usefully applied in the face of future changes. The current design uses a class ```Location``` that represents a cell's location in the world of Conway's game of life. This class assumes that locations are within an unbounded 2D grid and considers vertically, horizontally, and diagonally adjacent locations to be neighboring locations for the sake of applying Conway's rules for cell propagation across generations.

Imagine that we want to work with locations in a three-dimensional grid, or that we want locations that don't consider diagonally adjacent locations to be neighbors. My design doesn't conform to the OCP regarding locations, and so this would require changing exisiting code in the current design. If we took these changes as an opportunity to apply the OCP to locations, then we might implement a ```Locatable``` protocol that abstracts the behavior of my ```Location``` class and allows for different implementations. The original ```Location``` might become ```TwoDimensionalGridLocation``` and we could make our changes by writing a ```ThreeDimensionalGridLocation``` or a ```OrthagonalAdjacencyLocation``` that implement the ```Locatable``` protocol. Future variations on locations in Conway's game of life could similarly be added and used solely by writing new classes that implement ```Locatable```.

So, that's my understanding of the Open/Closed Principle. I hope you find it helpful.
