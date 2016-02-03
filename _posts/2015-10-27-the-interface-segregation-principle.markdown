---
layout: post
section-type: post
title: The Interface Segregation Principle
category: design
tags:
- design
- SOLID principles
---

The The Interface Segregation Principle (ISP) is a design principle with the aim to keep interfaces small and cohesive, thereby reducing/preventing coupling in code and offering focused areas of functionality that are easier to reuse. ISP is one of the five SOLID principles. You can read about some of the other SOLID principles here: the [Dependency Inversion Principle](http://scarvill91.github.io/design/2015/10/05/the-dependency-inversion-principle.html), the [Single Responsibility Principle](http://scarvill91.github.io/design/2015/10/21/the-single-responsibility-principle.html), and the [Open/Closed Principle](http://scarvill91.github.io/design/2015/10/22/the-open-closed-principle.html).

The ISP is commonly formulated as:

> "Clients should not be forced to depend upon interfaces that they don't use." ([source](http://www.oodesign.com/interface-segregation-principle.html))

This certainly makes intuitive sense - why have dependencies when they can be avoided? The ISP focuses on a common scenario - when we need only a subset of the functionality that an object, class, etc. makes available. We are forced to bring all of the functionality into play, but leave much of it unused. We protect ourselves from creating this kind of unnecessary dependency by segregating the functionality into interfaces that only expose the subsets of behavior we actually need and using those instead.

In compiled languages, this can save the need to recompile and redeploy elements that are unrelated to the changes we make. This also reduces the potential for coupling between distinct areas of functionality in our code in general.

It can be hard to distinguish between the application of the ISP versus the Single Responsibility Principle (SRP) in a language that doesn't have explicit interfaces like Java or Swift. I've found this to be true for at least for myself, being most familiar with Ruby. There are a lot of statically-typed demonstrations of the ISP, however, so I'm going to do my best to illustrate the ISP and contrast it with the SRP using an example in Ruby.

I give credit to a [stackoverflow answer](http://stackoverflow.com/questions/8099010/is-interface-segregation-principle-only-a-substitue-for-single-responsibility-pr) by [anotherdave](http://stackoverflow.com/users/1474421/anotherdave) and a [blog post](http://marioaquino.blogspot.com/2013/07/interface-segregation-principle-in-ruby.html) by Mario Aquino for providing the basis for my example.

The example concerns the implementation of a ```Car``` class. A SRP-compliant version of ```Car``` would likely package aspects of its implementation into subclasses or something similar. For example, a ```change_gear``` method in ```Car``` might delegate the particulars of changing gears to a ```Gearbox``` class. No matter how we package up the discrete components of ```Car``` according to the SRP, however, all of the functionality ultimately available will be through the public interface of ```Car```.

The ISP invites us to consider the cohesiveness of ```Car```'s interface. It is quite possible that the clients of ```Car``` use it for very different things, and therefore only require access to subsets of ```Car```'s functionality. For example, there might some clients who depend on ```Car``` as drivers - using methods like ```fasten_seatbelt``` or ```change_gear``` - and other clients who depend on ```Car``` as mechanics - using methods like ```adjust_tire_pressure``` or ```replace_spark_plug```.

<pre style="text-align: left">
class Car
  def fasten_seatbelt
  end

  def change_gear
    # might delegate to Gearbox
  end

  def adjust_tire_pressure(change_in_psi)
    # might delegate to Tire
  end

  def replace_spark_plug
    # might delegate to Engine
  end

  def open_door
  end

  def start_engine
    # might delegate to Engine
  end
end
</pre>

In this kind of scenario, we could apply the ISP to separate the methods required by these different kinds of clients into ```DriveableCar``` and ```MaintainableCar```. Methods used by both kinds of clients, such as ```open_door``` or ```start_engine```, would be included in both interfaces.

<pre style="text-align: left">
require 'forwardable'

class DriveableCar
  extend Forwardable
  def_delegators :@car, :fasten_seatbelt, :change_gear, :open_door, :start_engine

  def initialize(car)
    @car = car
  end
end

class MaintainableCar
  extend Forwardable
  def_delegators :@car, :adjust_tire_pressure, :replace_spark_plug, :open_door, :start_engine

  def initialize(car)
    @car = car
  end
end

# to initialize:
DriveableCar.new(Car.new)
MaintainableCar.new(Car.new)
</pre>

Notice that we could also apply the SRP in the same situation to break up the ```Car``` class and reduce its interface that way.

<pre style="text-align: left">
class Car
  def open_door
  end

  def start_engine
  end
end

class DriveableCar < Car
  def fasten_seatbelt
  end

  def change_gear
  end
end

class MaintainableCar < Car
  def adjust_tire_pressure(change_in_psi)
  end

  def replace_spark_plug
  end
end
</pre>

Like in this example, the SRP can usually be applied wherever you apply the ISP. The two principles encourage viewing the problem in different ways, however, and pragmatic considerations can lead you to favor one over the other. If, for example, I was unable to alter the source code for the Car class, segregating the interface as in the ISP example might be my only option.
