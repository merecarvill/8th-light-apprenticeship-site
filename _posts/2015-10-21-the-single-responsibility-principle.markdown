---
layout: post
section-type: post
title: The Single Responsibility Principle
category: design
tags:
- design
- SOLID principles
---
The Single Responsibility Principle (SRP) is one of the SOLID principles of good object-oriented design. For backrgound information about the SOLID principles, view my [post on the Dependency Inversion Principle](http://scarvill91.github.io/design/2015/10/05/the-dependency-inversion-principle.html). The SRP strikes me as the most deceptive of the SOLID principles, in the sense that I found it very easy to arrive at an incorrect or incomplete understanding of the principle. This is entirely due to the seeming intuitiveness of the name 'Single Responsibility.' What more do you even have to understand beyond the name? It sounds reasonable that things in an object-oriented design should have only one responsibility - it sounds simple and clean.

The reality is not so simple, and that's the pitfall with the SRP. At least the bogglement I felt when I first heard the term 'Liskov Substitution Principle' (another of the SOLID principles) prevented me from forming any mistaken intuitions about what the principle entails and encouraged me to give it a proper investigation. For one, what counts as a 'responsibility,' and which elements in our code should we worry about only having one of them? In addition, the SRP may encourage designs with smaller classes and method definitions, but there will be a correspondingly greater number of them - so the resulting code is not necessarily 'simpler.'

So, the pitfalls reagarding understanding the Single Responsibility Principle - for me at least - were the halfbaked intuitions I developed based on its name. The widely accepted definition of 'responsibility' to mean 'reason to change' in this case helped to refine my understanding of the SRP ([1](http://www.objectmentor.com/resources/articles/srp.pdf)) - the 'Single Reason to Change Principle' wouldn't have quite the same ring to it, but is a little more transparent about the intended meaning. To discuss the concretes of what constitutes a 'reason to change,' I'd like to go over some examples of applying (and failing to apply) the SRP in my own code.

In implementing [Conway's Game of Life](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life) in swift, I settled on two classes representing abstractions in my design. ```World``` represents cells in an infinite grid and creates new worlds containing subsequent generations of cells produced according to Conway's rules for cell propagation. ```Location``` represents a single location in the infinite grid.

<pre style="text-align: left">
public class World {
    public init(livingLocations: [Location] = default)
    public func advanceGeneration() -> GameOfLife.World
    public func aliveAt(location: Location) -> Bool
}

public class Location : Equatable, Hashable {
    public init(coordinates: (Int, Int))
    public var hashValue: Int { get }
    public var neighbors: [GameOfLife.Location] { get }
}
public func ==(lhs: Location, rhs: Location) -> Bool
</pre>

I'll start with what I consider to be some successful applications of the SRP. The separation of ```Location``` from ```World``` is good in a few ways. ```World``` treats ```Location``` instances as black boxes, trusting only that they can be compared for equality and can return a list of their neighbors. In this way, ```Location``` abides by the SRP by encapsulating one (and only one) potential source of change. If we want to change our representation of cell locations, to exist in a 3D rather than 2D grid for example, the only necessary changes would be to the ```Location``` class.

The SRP can be applied to methods/functions as well. Within ```World```, I determine which living cells persist into the next generation using the following methods:

<pre style="text-align: left">
func persistingLocations() -> [Location] {
  return livingLocations.filter({ notUnderpopulated($0) && notOverpopulated($0) })
}

func notUnderpopulated(location: Location) -> Bool {
  return numberOfLiveNeighbors(location) >= 2
}

func notOverpopulated(location: Location) -> Bool {
  return numberOfLiveNeighbors(location) <= 3
}

func numberOfLiveNeighbors(location: Location) -> Int {
  return location.neighbors.filter({ aliveAt($0) }).count
}
</pre>

**A note about the implementation:** ```World``` tracks the locations of all living cells in the variable ```livingLocations```. There's no actual representation of cells with a living or dead state, since it turns out it's not necessary.

Whether this is an ideal implementation or not, I think I did an acceptable job of writing simple, small methods that will likely only have one reason to change in the future. ```notUnderpopulated()``` would likely only change if the numeric rules for determining 'underpopulation' were to change, ```persistingLocations()``` would likely only change if further (or fewer) rules needed to be taken into account to determine a cell's survival into the next generation, etc.

As a class, however, ```World``` strikes me as likely to change for a handful of different reasons. You might want to try some variations on the rules of Conway's game of life, you might want to change the underlying implementation of you track cells' state and their location relative to each other, or you might want to change the process by which sucessive generations of cells are decided and produced. In this way, ```World``` fails to abide by the SRP. Separating the logic specific to the game of life rules into a separate class or structure would go a long way towards remedying this.

Hopefully the above examples go a ways towards demonstrating what the SRP means by "only one reason to change." Applying the Single Responsibility Principle is ultimately a pragmatic consideration. It's up to our imagination and judgement to consider the distinct ways that our code might later change and the likelihood of each change. The SRP often helps to produce code that consists of small, discrete components that promote overall flexibility and clarity. However, it also usually adds to the overall complexity of your code, and in some cases the code may be unlikely enough to change or easy enough to change that it's not worth it.

I haven't addressed the question of "what elements in code should the SRP be applied to?" mostly because I'm hard pressed to think of what elements wouldn't benefit from it - be it a 'for' loop or a library.
