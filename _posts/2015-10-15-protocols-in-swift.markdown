---
layout: post
section-type: post
title: Protocols in Swift
category: swift
tags:
- swift
---

### The Backstory

I've been working on implementing Conway's game of life in swift as a means of picking up the language. I'd reccommend looking into the game of life if you're unfamiliar with it. It's basically a simple set of rules simulating the death and reproduction of cells, situated in an infinite 2D grid, across sequential generations. The simple premise gives rise to some surprising complexity - perhaps the most impressive example being the implementation of [Conway's game of life within Conway's game of life](http://jeremykun.com/2011/11/03/conways-game-of-life-in-conways-game-of-life/)!

Anyway, I was using integer tuples to represent coordinates within the two-dimensional space of the game of life, but soon found myself frustrated that a number of stock functions in Swift wouldn't work with these tuples. For example, Swift didn't know how to compare two such tuples for equivalence. Frustrating as that is alone, that also prevented me using any methods that relied on an existing ```==``` method - such as ```contains```, which checks if an array contains the given element.

To address this, I implemented a class to represent locations in the 2D grid and set about figuring out how I was going to get Swift to understand how to compare instances of *that* for equivalence. To do that, I had to learn about protocols - which are a nifty way of defining functionality for elements in Swift that conform to a set of requirements.

### The Equatable Protocol

In the case of the ```Equatable``` protocol, I needed to implement a global function ```==``` that worked for my Location class.

<pre style="text-align: left">
func ==(lhs: Location, rhs: Location) -> Bool {
  return lhs.row == rhs.row && lhs.col == rhs.col
}
</pre>

With that done, I simply had to indicate that my class conforms to the ```Equatable``` protocol, like so:

<pre style="text-align: left">
class Location: Equatable {
  // ...
}
</pre>

And boom! I could finally do things like check if an array of Locations contained a particular Location. Pretty nice, huh?

### It's a Wide World of Protocols

As you might guess, you can write your own protocols - which makes them quite a powerful tool for extracting shared behavior in your code. I don't want to get too far into custom protocols in this post, but here's an example of how you'd implement a protocol that has a required initializer, property, and method:

<pre style="text-align: left">
protocol SomeProtocol {
  required init(someParameter: SomeType) {
  }
  static var requiredProperty: SomeType { get set }
  static func requiredMethod()
}
</pre>

Swift's standard library also comes stocked with a number of useful protocols, which you can check out [here](https://developer.apple.com/library/watchos/documentation/General/Reference/SwiftStandardLibraryReference/index.html#protocols). I ended up conforming to ```Hashable``` with my Location class as well, in order to be able to use a location as a key in a dictionary. That required that I create a property ```var hashValue: Int { get }``` for my Location class - which was easily done by xor-ing together the ```hashValue``` properties of a Location's ```row``` and ```col``` properties (as Integers, they already had ```hashValue``` defined).