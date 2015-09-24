---
layout: post
title:  Connascence
date:   2015-09-23
categories: testing
---
Originally introduced by Meilir Page-Jones' *[What Every Programmer Should Know About Object-Oriented Design](http://www.amazon.com/Every-Programmer-Should-Object-Oriented-Design/dp/0932633315)*, connascence is a helpful concept that helps us to consider dependencies in our code and their relative impact on the flexibility of our designs. Two elements are connascent when a change in one element requires a change in the other in order to preserve the correct behavior of the system.

# Types of connascence

**Connascence of Name** occurs when multiple elements rely on shared knowledge of a name. Method names are a common example of this form of connascence: if the name of a method changes, callers of that method must be changed to use the new name.

**Connascence of Type** occurs when multiple elements rely on shared knowledge of a type. For example, if a method expects a string as an argument and is later changed to expect an integer, callers of that method must be changed to use the new type. Ditto for changes in return types.

**Connascence of Meaning** (also known as Connascence of Convention) occurs when multiple elements rely on shared knowledge of the meaning of a particular value. Magic strings/numbers are examples of this form of connascence, such as code that uses '0' to indicate success and '1' to indicate failure.

**Connascence of Position** occurs when multiple elements rely on shared knowledge of a particular order of values. The most common example of this are positional parameters. A change in the order of a method's parameters requires a corresponding change everywhere the method is called.

**Connascence of Algorithm** occurs when multiple elements rely on shared knowledge of a particular algorithm. For example, this could occur in code that encrypts and decrypts data if different elements of the code share assumptions about the particular encryption algorithm used.

**Connascence of Execution** occurs when multiple elements rely on shared knowledge of the order of execution of multiple elements. For example, you can have a method that will not work correctly unless a different method has been called first.

**Connascence of Timing** occurs when multiple elements rely on shared knowledge of the timing of the execution of multiple elements. This could be a method that requires another method be called a set time later or once a particular change in state has taken place (and relies on something else to do the calling).

**Connascence of Value** occurs when multiple elements rely on shared knowledge of how a value must change. For example, elements of an application that know that 'days since last accident' should be (or will be) reset to zero when a workplace accident occurs.

**Connascence of Identity** occurs when multiple elements rely on shared knowledge of the reference to a particular entity. This could occur if you had two objects that share a reference to a third object - such as two objects reading from and writing to the same array.

# Considerations

The few blog posts that I've read on connascence have ranked the types in terms of the cost and difficulty that each impose on future changes. The above types are listed in the same order of increasing severity as listed in those blogs. Page-Jones makes no mention of relative severity in the original book, however, and also lists the types of connascence in this same order.

That said, instances of connascence can assuredly vary in the degree to which they hamper future changes to code. This certainly does differ (on average) between types. For example, connascence of name is likely less severe on average than connascence of algorithm - it should generally be easier to find and alter instances of a given name than to identify and figure out all the areas in which code must be changed in response to a change of algorithm.

Page-Jones also stresses that the above list is not comprehensive - there are likely other types of connascence beyond those ennumarated above.

