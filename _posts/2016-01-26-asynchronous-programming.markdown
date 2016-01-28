---
layout: post
section-type: post
title: Asynchronous Programming
category: learning
tags:
- learning
---
Synchronous programming is what most people think of when they consider programming in general. Instructions are executed sequentially, each executed after the previous one has been completed. Asynchronous programming, on the other hand, occurs when instructions can be executed before others have been completed.

Most people agree that synchronous programming is generally much easier to comprehend than asynchronous code - for the simple reason that one thing happening at a time in a determinate order is much easier to keep track of than several things happening simultaneously and in an indeterminate order.

If you're familiar with the concept of [threads](https://en.wikipedia.org/wiki/Thread_(computing)), realize that you do not need multiple threads to execute asynchronous code. A single thread can start, pause, and swap between multiple processes - executing only one at a time, but in indeterminate order and over an indeterminate period of time. Asynchronous programming does however work very well with multiple threads, since that allows instructions to be executed in parallell - in addition to being allowed to start before others have finished.

Asynchronous programming is well suited to many applications in computing, especially when:

* There are a large number of tasks, since there is usually at least one task that can make progress at any given time.
* There are tasks that perform lots of I/O operations, each of which would waste lots of time waiting for these relatively slow operations to complete when
other tasks could be running.
* There are tasks that can be completed mostly independently from one another, and thus don't really need to wait for each other for inter-task communication.

My introduction to asynchronous programming occurred quite recently through the development of an iOS app that depends on calls to a web API. Everyone these days relies on asynchronous programming for network communications, since it is often quite slow and waiting for it to complete poses a significant performance bottleneck for most network-dependent applications.

In my case, I spent a rather trying period of time wrapping my brain around the concept of callbacks. Callbacks are one way of handling asynchronous code. Put simply, you give a task another task to execute after the first task is finished. This allows you to ensure that a task will only start after another task has been completed, which is helpful if you have some tasks that must be completed in a particular order in an asynchronous system.

This is the code I ended up using to make asynchronous calls to the web API in my iOS app:

<pre style="text-align: left">
public func makeRequest(
    url: String,
    successHandler: (String) -> Void,
    failureHandler: () -> Void
    ) {
    let session = NSURLSession.sharedSession()
    let request = NSURLRequest(URL: NSURL(string: url)!)

    session.dataTaskWithRequest(request) { (data, response, error) -> Void in
        if error == nil && data != nil {
            let reply = String(data: data!, encoding: NSUTF8StringEncoding)
            successHandler(reply!)
        } else {
            failureHandler()
        }
    }.resume()
}
</pre>

The callback in the above snippet is the block being passed to ```dataTaskWithRequest```. I still don't understand nearly everything about how ```NSURLSession```s and their associated tasks operate, but calling ```dataTaskWithRequest``` on a ```NSURLSession``` creates a representation of an asynchronous network call that can then be started by calling ```resume```. When the call completes, the resulting data is passed to the callback defined within the block.

I wrote the above method in an effort to abstract away as much of the process of making an asynchronous network call as I could. In this case, when calling this method, all I had to worry about were knowing the URL I was targeting and defining the behavior I wanted should the network call succeed or fail.

Other than callbacks, there are many other concepts that have been developed for handling asynchronous code. These include [promises](https://en.wikipedia.org/wiki/Futures_and_promises) and [generators](https://en.wikipedia.org/wiki/Generator_(computer_programming)) - each of which easily warrant their own blog post. In brief, however:

Promises represent either a value that will be successfully caluculated in the future or the eventual failure to do so. As such, you can define two potential paths based on promises: a sequence of tasks that depend on the promised values returned by their predecessors as well as what you want to happen should a promise in the chain fail at any point.

Generators implement a specialized form of iteration. For example, a simple generator could be an object with a method that returns an integer value that increments by one every time the method is called. Generators are often used as a convenient means of representing infinite or lazily-evaluated sequences, as in the infinite sequence of incrementing integres in the example I gave. In a similar vein, generators can be used to represent a group of interchangeable future values, and a task that uses those values can choose to retrieve (and possibly wait) for the next value to be determined at any point in its execution. This abstracts away the fact that these values are evaluated in indeterminate order and in an indeterminate amount of time. Instead, generators present an easy-to-use, sequential interface.

There are still many more concepts related to handling asychronous code beyond those of callbacks, promises, and generators. I've just begun to scrape the surface myself. What I've learned so far, however, has been more than sufficient to convince me that developing a strong understanding of asynchronous programming concepts is an essential step to considering myself a software craftsman. It has incredibly broad and powerful applications throughout software development - especially within mobile and web application development.
