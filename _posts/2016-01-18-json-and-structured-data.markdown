---
layout: post
section-type: post
title: JSON and Structured Data
category: data
tags:
- data
---
I'm working on implementing a simple web API that provides the moves for an unbeatable computer player to an iOS tic tac toe app. While the design is a little contrived, it's been an excellent educational scenario set out for me by my mentors at 8th Light. Among several firsts - such as putting the http server I previously created to use and learning the basics of iOS - I'm now confronted for the first time with JSON and the concept of structured data. This is because I need to send the relevant data about a tic tac toe game in progress to my web API, and to send back an appropriate move for the unbeatable computer player. In such situations, when you want to transmit data between a server and another application, it behooves developers to organize the transmitted data in a format that allows simple and easy access once it is received at its destination.

This is accomplished by structuring data. Structured data has a standardized relationship between its component elements that allows for the precise representation of a certain domain of information. Semi-structured data is a companion term that describes structured data that does not abide by the formal structure of relational databases or other table-based data models, yet encodes a standardized relationship between its elements by some other means. This latter term is generally considered to include JSON (JavaScript Object Notation) and markup languages, such as XML (Extensible Markup Language).

JSON is a language-independent data format that is popularly used for stateful, real-time server-to-browser communication. JSON structures its data through the use of attribute-value pairs. Attributes are always strings. Values can be:

- A number, e.g. ```"age": 30```,
- A string, e.g. ```Happy Birthday!",
- A boolean, e.g. ```true``` or ```false```,
- An array, e.g. ```"inventory": ["cereal": 5, "milk": 2, "eggs": 12]```
- An object, e.g.```{ "firstName": "George", "lastName": "Washington", "isAlive": false, "yearBorn": 1732 }```
- or ```null```

Arrays are ordered lists of values. Objects are unordered collections of attribute-value pairs.

My web API needs to know a tic tac toe game's current player and the existing marks on the game board in order to determine the correct move for an unbeatable computer player to make. Using JSON to structure this information, the body of an incoming http request might look like this:

<pre style="text-align: left">
{
  "currentPlayer": "X",
  "board": [
    "X", null, "O",
    "O", "X", "null"
    "X", "O", null
  ]
}
</pre>

JSON ignores whitespace, so you are free to indent elements in a way that makes them easier to read and understand.

I also need to send a chosen move in response to a request. I can identify the selected move any way I want, so long as I pick a convention and stick to it. In this case, I'll send the index of the space in the provided array that represents the game board. The winning move would be located at the final slot in the array, so the appropriate response is 8, since that is the final index of a 9-element array. The body of the http response sent in response might therefore look like this:

<pre style="text-align: left">
{
  "chosenSpace": 8
}
</pre>

Because JSON data is organized according to a standardized structure, there are libraries for pretty much any given language that allow easy access, generation, and manipulation of JSON data. Without this, it would be necessary to implement the necessary functionality for transmitting and parsing data via http both in my iOS tic tac toe app and in my web API. I haven't tried it out quite yet, but here's to hoping that structuring my data with JSON can save me some headaches.
