---
layout: post
section-type: post
title: The Builder Pattern
category: design
tags:
- design
---
The Builder design pattern is a design pattern for creating objects. In general, the Builder pattern allows the separation of a complex object's creation from its representation. The Builder pattern can be applied to simplify and streamline object creation when you would otherwise require several different constructors and/or when the constructors require a lot of parameters.

The pattern achieves this by delegating object construction to a separate "builder" object. The builder object accepts each initialization parameter one by one via a series of method calls. A final ```build``` method call then returns the constructed object.

## Background

In making an http server as an apprentice project, it was an easy decision to create Java objects to represent an http request and an http response. I translated between the objects and the formatted http text at the boundaries of my application, allowing the majority of the application logic to interact with these objects instead.

However, http requests and responses are composed of a number of discrete elements - methods, statuses, headers, a message body etc. - which made the initialization of the request and response objects somewhat complex. This provided me an opportunity to implement the Builder design pattern in order to simplify the creation process for request and response objects.

## The builder in action

The following is my implementation of a builder object for an http request:

<pre style="text-align: left">
public class RequestBuilder {
    private Method method;
    private String uri;
    private HashMap<String, String> parameters = new HashMap<>();
    private HashMap<String, String> headers = new HashMap<>();
    private byte[] body;

    public Request build() {
        return new Request(method, uri, parameters, headers, body);
    }

    public RequestBuilder setMethod(Method method) {
        this.method = method;
        return this;
    }

    public RequestBuilder setUri(String uri) {
        this.uri = uri;
        return this;
    }

    public RequestBuilder setParameter(String name, String value) {
        this.parameters.put(name, value);
        return this;
    }

    public RequestBuilder setHeader(String keyword, String content) {
        this.headers.put(keyword, content);
        return this;
    }

    public RequestBuilder setBody(byte[] body) {
        this.body = body;
        return this;
    }
}
</pre>

My implementation of ```ResponseBuilder``` is very similar. In my http server code, a ```Request``` is built like this:

<pre style="text-align: left">
new RequestBuilder()
    .setMethod(Method.GET)
    .setURI("/projects/1")
    .setParameter("username", "scerpen")
    .setHeader("Keep-Alive:", "timeout=15, max=100")
    .setBody("page body".getBytes())
    .build();
</pre>

I find this syntax very easy to follow compared to jamming all of these arguments into a constructor. The builder object also saves me some work in object construction. To set query string parameters or headers I don't need to go to the effort of assembling them into a ```HashMap``` to pass into the ```Request``` object constructor.

Last, the builder allows me to set only those fields that I need without polluting the ```Request``` definition with several overloaded constructors. Creating a  ```Request``` with only a method and URL would look like this:

<pre style="text-align: left">
new RequestBuilder()
    .setMethod(Method.GET)
    .setURI("/")
    .build();
</pre>

## Fluent interfaces

Classes implementing the Builder pattern are great candidates for a [fluent interface](https://en.wikipedia.org/wiki/Fluent_interface). A fluent interface, as in the ```RequestBuilder``` example above, allows sequential chaining of method calls that operate on an object.

For this to be possible, the return value of the chainable methods must be substitutable for the receiver of the called method. In the case of ```RequestBuilder```, each setter method returns a modified ```RequestBuilder``` object. The chain of method calls in a fluent interface is often terminated by a method whose return value is not substitutable for the recipient of the method call. ```RequestBuilder```'s ```build``` method returns a ```Response``` object, for example.

I enjoy this kind of interface and I'm happy to have a name for it now.
