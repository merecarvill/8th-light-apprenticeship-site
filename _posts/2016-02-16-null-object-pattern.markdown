---
layout: post
section-type: post
title: The Null Object Pattern
category: design
tags:
- design
---
The Null Object design pattern provides an alternative to the use of null references in programming (typically called ```nil``` or ```null``` in most languages). Whereas calling methods on null references generally results in a compile or runtime error, the idea behind the Null Object pattern is to instead implement an object with defined neutral ("null") behavior that can be treated identically to other objects with the same interface.

This can be a useful strategy for implementing polymorphic behavior in which you want some objects to do something and others to do nothing. Using null objects in place of null references can eliminate the need for explicit checks for null references before calling a method on an object.

In brief, a null object implements "do nothing" behavior - encapsulating this knowledge from clients and simplifying them in the process by removing the need for explicit null reference checks or associated branching logic.

To provide an example, imagine you wanted to send yearly reminders to subscribers to a periodical reminding them to renew their subscription. However, some subscribers have purchased a lifetime subscription and should not be sent a message.

You might loop through the subscribers, invoking the line ```subscriber.send_reminder if subscriber.instance_of YearlySubscriber```. Alternatively, you could implement the Null Object pattern by implementing a ```send_reminder``` method for ```LifetimeSubscriber```s and call ```subscriber.send_reminder``` on all subscribers - removing the need for type-checking.

<pre style="text-align: left">
class YearlySubscriber
  def send_reminder
    # send subscriber an email reminding them to renew their subscription
  end
end

class LifetimeSubscriber
  def send_reminder
  end
end
</pre>

In a statically-typed language, such as Java, it is common for null objects to implement an explicit interface shared with other objects (or to inherit from a common abstract class). To demonstrate:

<pre style="text-align: left">
public interface Subscriber  {
  public void sendReminder()
}

public class YearlySubscriber implements Subscriber {
  public void sendReminder() {
    # send subscriber an email reminding them to renew their subscription
  }
}
public class LifetimeSubscriber implements Subscriber
  public void sendReminder() {}
}

for (Subscriber subscriber : subscribers) {
  subscriber.sendReminder()
}
</pre>

The Null Object pattern compliments some other design patterns. A null strategy - one that simply does nothing when it is applied, among the strategies implemented using the Strategy pattern. Visitors in the Visitor pattern also do not necessarily have to do something for every type of object they visit. In this scenario, some types of visited elements may emulate null objects by doing nothing when ```accept```ing a visitor.
