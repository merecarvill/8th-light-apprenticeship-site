---
layout: post
section-type: post
title: The Dependency Inversion Principle
category: SOLID
tags:
- SOLID
- design
---
The Dependency Inversion Principle (DIP) is one of five principles of good object-oriented design. You may recognize it as the "D" in "[SOLID](https://en.wikipedia.org/wiki/SOLID_(object-oriented_design))" - Michael Feathers' widely-known acronym for the five principles. Applying the DIP in conjunction with the other SOLID principles aims to produce systems that are easier to maintain and extend. The DIP hinges on the idea that abstractions are less subject to change than the details of their implementation. A system is therefore easier to change when the elements containing the details of implementation depend on elements that are relatively more abstract. The formulation of the DIP as it first appeared in Uncle Bob's [Agile Software Development: Principles, Patterns, and Practices](http://www.amazon.com/Software-Development-Principles-Patterns-Practices/dp/0135974445) is as follows:

> A. High-level modules should not depend on low-level modules. Both should depend on abstractions.
> B. Abstractions should not depend on details. Details should depend on abstractions.

### What is a dependency?

From the perspective of any given element in a system, a dependency describes something that the element "depends" on. The dependent element is, unsurprisingly, called the "dependent." An example of a very common type of dependency is dependency on the interface of any given programming language. In Ruby, this could be a dependency on Array objects, on associated methods like #size or #map, or on the syntax for defining blocks. This also serves to illustrate that dependencies are inescapable and not always terrible. The API of a programming language is one of the better things to for code to depend on because - relatively speaking - it is likely to change infrequently, in small increments, and, even then, will usually be backwards compatible. My previous post on Connascence is a good reference for considering various forms of dependency.

### A helpful metaphor

I think the popular metaphor of lamps and plugs illustrates the DIP well. When you want to power a lamp, you don't have to directly access your home's wiring and solder it together with the lamp's power cord. Instead, you have your choice of a handful of identical sockets into which you can plug your lamp - you don't have to interact with the undelying wiring at all. The DIP aims to produce systems that emulate this sort of convenience and flexibility. If you translate the metaphor into the terms of Uncle Bob's definition of the DIP, the lamp is a high-level module, the wiring is a low-level module, and the socket is an abstraction - in this case, of the energy provided by the wiring divorced from any messy details.

### A simple example

Let's try to translate this metaphor into an example in Ruby. Let's say we have Circuits that ```conduct_electricity```, but must be wired up beforehand in order to be successful.

<pre style="text-align: left">
class Circuit
  @wired_up = false

  def wire_up
    @wired_up = true
  end

  def conduct_electricity
    :success if @wired_up
  end
end
</pre>

Let's say we also have Lamps that can ```get_power``` from a circuit that can sucessfully ```conduct_electricity```.

<pre style="text-align: left">
class Lamp
  @powered = false

  def get_power(circuit)
    circuit.wire_up
    @powered = true if circuit.conduct_electricity == :success
  end
end
</pre>

As we have implemented them, Lamps are heavily dependent on Circuits - the knowledge that a Circuit must be wired up before it can successfully ```conduct_electricity``` is explicitly encoded in Lamp's ```get_power```. This is the equivalent to having to monkey around directly with circuitry in order to get a lamp to work, rather than being able to plug it into any ol' socket. If the way that Circuits conduct electricity changed, then the design of a Lamp would likely have to change in reponse. If you wanted to use a different power source that operates differently from Circuits - such as Batteries - you'd also have to change your design of Lamp. That sucks, but applying the DIP can help us.

First, we have to realize an abstraction in our code. We don't actually care about having a Circuit ```conduct_electricity``` for a Lamp. We just want a power source (defined concretely as any object that can ```provide_power```). This abstract concept of a power source that responds to ```provide_power``` is the equivalent of the socket from the metaphor. This concept would need to be implemented explicitly in a statically-typed language, but in Ruby it just needs to exist as a convention in our design. We can remove Lamp's dependency on Circuits by changing both Lamps and Circuits to depend on this abstraction instead.

<pre style="text-align: left">
class Circuit
  @wired_up = false

  def provide_power
    wire_up
    conduct_electricity
  end

  private

  def wire_up
    @wired_up = true
  end

  def conduct_electricity
    :success if @wired_up
  end
end

class Lamp
  @powered = false

  def get_power(power_source)
    @powered = true if power_source.provide_power == :success
  end
end
</pre>

This may seem like a relatively trivial change, but the ramifications are significant. For example, I can create a Battery class that can ```provide_power``` to a Lamp without any modification of the existing design - despite the fact that the implementation of how a Battery provides power is completely different from that of a Circuit.

<pre style="text-align: left">
class Battery
  @charges = 4

  def provide_power
    recharge if depleted?
    discharge
  end

  private

  def discharge
    :success unless depleted?
    @charges -= 1
  end

  def depleted?
    @charges == 0
  end

  def recharge
    @charges = 4
  end
end
</pre>

Not only would you have to change the old design of Lamps to work with Batteries, but you would probably end up with an ugly series of ```if``` ```else``` clauses or even ```case``` statements if you continued adding power sources without applying the DIP. Like this, for example:

<pre style="text-align: left">
class Lamp
  @powered = false

  def get_power(power_source)
    switch power_source
    case power_source.kind_of? Circuit
      power_source.wire_up
      @powered = true if power_source.conduct_electricity == :success
    case power_source.kind_of? Battery
      power_source.recharge if power_source.depleted?
      @powered = true if power_source.discharge == :success
    end
  end
end
</pre>

Ew. Now just consider how tangled things could get in a non-trivial example.

### Conclusion

And that's the Dependency Inversion Principle folks! I've found it helpful in the design of my Tic Tac Toe application thus far. I've implemented both my ComputerPlayer and HumanPlayer to abide by the same abstract "player" interface, which frees my game from having to behave any differently based on which type of player it is interacting with. I'm also anticipating applying the DIP to the Tic TacToe user interface as I prepare to add a Rails-based interface alongside the existing command line interface. I'm confident you'll find the DIP useful in many instances as well.