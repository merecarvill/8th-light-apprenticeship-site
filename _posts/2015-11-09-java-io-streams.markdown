---
layout: post
section-type: post
title: Java IO Streams
category: java
tags:
- java
---
An input/output (IO) stream is a powerful abstraction that represents a source of continuous data entering or exiting a program. The source or destination of data can be one of a wide range of entities - such as a file on disk, a network connection, a keyboard or any peripheral device, or another program.

All data is transmitted as binary data, allowing any kind of data to be sent between your program and another entity - so long as you know what kind of data to expect, so that it can be correctly interpreted from its underlying binary form. Many of the Java classes relating to IO streams are intended to make sending and receiving data of a particular type easier by handling the translation of that datatype to and from its binary from.

### Byte Streams

Java byte stream classes handle the fundamental IO of raw binary data, parsing it in increments of one byte. All Java byte stream classes inherit from the ```InputStream``` and ```OutputStream``` classes. Data is received via ```InputStream``` and its descendants by calling the ```read``` method and is sent via ```OutputStream``` and its descendants by calling the ```write``` method.

Both methods can raise an ```IOException``` in the case that an IO operation is interrupted or fails. All IO streams also require that you call the ```close``` method to signify that you are finished with IO operations and to allow the system to free up the associated resources.

### Fancier Streams

Some other stream classes that extend the functionality of byte streams are character streams, data streams, and object streams. These classes are wrappers for a byte stream, although you don't always need to specify one at initialization. In each case, they offer greater functionality directed towards sending and receiving different kinds of data. Character streams read and write data in increments of one character and handle translating between character encoding and its binary form. Character streams inherit from the abstract ```Reader``` and ```Writer``` classes. Some character stream classes offer additional functionality, such as reading text a line at a time using ```BufferedReader``` or writing a line at a time using ```PrintWriter```.

Data streams handle parsing binary data as any primitive datatype in Java and implement the ```DataInput``` or ```DataOutput``` interface. These interfaces have read and write methods for every type of data - e.g. ```readChar```, ```writeInt```, ```readBoolean```, ```writeDouble```, etc.

Object streams handle parsing objects to and from binary data, although they can also handle reading and writing primitive datatypes as well. The object streams are ```ObjectInputStream``` and ```ObjectOutputStream```, which implement the ```ObjectInput``` and ```ObjectOutput``` subinterfaces of ```DataInput``` and ```DataOutput```. ```writeObject``` and ```readObject``` are the methods used to send and receive objects. ```writeObject``` handles the tricky matter of determining the full chain of references from the written object to other objects, and references from those objects to *other* objects, and so on.  All of these objects are then written to the stream. In this way, the object should behave as expected once it, and all of its referents, have been read from the stream. It is also useful to note that writing the same object to the same stream multiple times cannot result in the object being read from the stream more than once.

### Buffered streams

Streams can also be *buffered* or *unbuffered*. Every read and write to an unbuffered stream is immediately handled by the underlying operating system, which can trigger time-consuming actions like disk or network access. Buffered streams read and write to a space allocated in system memory and only delegate to the OS when the buffer is empty (in the case of reads) or full (in the case of writes). In Java, ```BufferedInputStream``` and ```BufferedOutputStream``` are buffered byte streams and ```BufferedReader``` and ```BufferedWriter``` are buffered character streams. Each is implemented as a wrapper of the corresponding unbuffered stream.