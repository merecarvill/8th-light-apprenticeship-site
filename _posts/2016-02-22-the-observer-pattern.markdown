---
layout: post
section-type: post
title: The Observer Pattern
category: design
tags:
- design
---
The Observer Design Pattern outlines one way of communicating changes in an object's state to its dependents. The dependent objects are called "observers" and the object under observation is called the "subject."

In the pattern, the subject possesses references to each observer and notifies them whenever the subject's state changes. This notification typically consists of invoking a method (usually named ```update``` or ```notify```) belonging to each observer.

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/8d/Observer.svg/854px-Observer.svg.png" alt="Observer Pattern diagram" width="100%">

The Observer Pattern is a good fit for event-driven systems. Such systems must respond dynamically to external events as part of normal function. Some examples of events include user actions (such as mouse clicks and key presses), sensor outputs, and interactions with other programs or threads. Event-driven systems include graphical user interfaces, web applications, and other systems which must handle events in real-time.

There are two primary means by which data is passed between the subject and observers in the Observer Pattern: the "push" model and the "pull" model.

In the push model, a subject sends all necessary information to observers whenever a change in state occurs.

The push model is the accepted solution when observers want data about changes in the subject every time they occur and as soon as they occur. For example, a text editing application typically needs information about every key press immediately as they occur.

In the pull model, a subject provides observers with a minimal notification of changes in state and leaves it to observers to gain further information via one or more subsequent queries of the subject.

The pull model is best used when observers consume data at a slower rate than is produced by the subject or when observers only need the data in certain situations. In these cases, observers are able to pull data from the subject as it is needed. For example, an app like Google Maps might poll your phone for GPS information less frequently than incoming GPS signals actually update your position in order to improve battery life and performance.

An example implementation of the Observer Pattern in Ruby follows. The code models a thermostat (subject) and a heater (observer). The thermostat periodically checks the temperature and notifies the heater if the temperature has changed. The heater then turns on or off when notified, based on whether the temperature is below the heater's target temperature.

<pre style="text-align: left">
class Thermostat
  def initialize
    @observers = []
  end

  def register(observer)
    @observers << observer
  end

  def notify_observers
    @observers.map{ |o| o.notify(@temperature) }
  end

  def monitor_temperature
    loop do
      sleep(1)
      old_temperature = @temperature
      @temperature = get_current_temperature()
      notify_observers() if old_temperature != @temperature
    end
  end
end

class Heater
  def initialize(target_temperature)
    @target_temperature = target_temperature
  end

  def update(current_temperature)
    if current_temperature < target_temperature
      # activate heater
    else
      # deactivate heater
    end
  end
end
</pre>

Any number of heater objects could be registered as observers of the thermostat - each object perhaps being responsible for controlling the heating of individual rooms in a house. Any number of additional types of objects could also observe the thermostat, so long as they implement the ```notify``` method - such as an object controlling an air conditioning unit.
