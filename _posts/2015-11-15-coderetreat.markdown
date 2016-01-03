---
layout: post
section-type: post
title: Coderetreat
category: learning
tags:
- learning
---
On November 14th I had the pleasure of participating in a global day of Coderetreat.
I had previously attempted a solo sort of coderetreat, at the suggestion of my mentor and prompted by having read Corey Haines' [Understanding the Four Rules of Simple Design](https://leanpub.com/4rulesofsimpledesign).
However, this was my first time at a proper coderetreat.

## Background

I'd once loosely imagined coderetreats to be something like a couple's retreat - except for improving programmers' relationship to code.
Trustfalls, but with segfaults?
I don't know.
Turns out coderetreats are a very particular flavor of deliberate practice for developers.
From what I understand, Corey Haines has been a major influence in popularizing the practice by facilitating many coderetreat events.

Codereatreats are day-long events in which pairs of programmers attempt to implement Conway's Game of Life in several short sessions.
The focus is on learning new patterns of development and improving design skill.
Pairs attempt to write their implementation according to constraints that are intended to push them out of typical approaches.
They delete their code after every session, and discuss the lessions learned as a group before beginning a new session.
More details about coderetreats are available [here](http://coderetreat.org/facilitating/structure-of-a-coderetreat).

## The event

The event was hosted by 8th Light, so I was able to participate in conducting things - greeting folks as they arrived, setting up the room and clearing it out at the end.
That gave me the distinction of delaying Haines from attending his signature event for a full half hour by accidentally tying up the building intercom.
I'm still kicking myself for that - sorry Corey!

There were people from all sorts of backgrounds in attendance: students, veteran developers, people who transitioned away from writing code for a living.
It's a credit to the format of coderetreats that the sessions easily accomodated everyone, regardless of their skills and inclination.
Conway's Game of Life is also a great subject for sessions.
The problem is easily explained, can be solved with a variety of approaches, and is difficult enough that it would be quite unlikely to code a complete solution during a session.

## Experiences

My first session was largely spent introducing my partner to the practice of TDD, and distilling the game rules down to the surprisingly simple boolean string ```numLiveNeighbors == 3 || (numLiveNeighbors == 2 && isAlive)```. This is all that is needed to decide if an individual cell will be alive in the next generation. ```numLiveNeighbors``` is the number of living cells orthogonally or diagonally adjacent to a given cell, and ```isAlive``` is simply a boolean value indicating if the given cell is alive.

My second session included the following constraints: no if/else branching, and no methods longer than 4 lines.
I let my partner lead the design of our application, and we arrived at a cool sort of imperative design that did a good job encapsulating the individual rules of the game.
We created an interface called ```Rule``` with the method ```apply``` which would alter the alive/dead state of a cell to reflect its state in the next generation.
For example, the class ```UnderPopulationRule``` implemented ```Rule```, with the method ```apply(Cell cell, List<Cell> neighbors)```.
The ```apply``` method filtered the list of neighbors to just the living neighbors and then changed the state of the given cell like so: ```cell.setIsAlive(liveNeighbors.size() < 2)```.
This style of design made it easy to change individual rules, since each was encapsulated within its own class, and made creating a new rule as simple as creating a new implementation of the ```Rule``` interface.

My third session was challenging, since my partner and I were under the "baby steps" constraint: we had to write code within very short timeboxes (2-3 minutes) and delete any code that was not passing all tests and under version control when the time was up.
Turns out we had to delete our code a lot.
By the end of our session, we had barely arrived at a cell abstraction with an alive/dead state and which tracked its location relative to other cells.

I took the lead in designing the application for my fourth session. For this session, we were not allowed to pass primitive values across class boundaries - including in constructors.
This also turned out to be quite a challenge.
In fact, it was very hard even to notice when we violated this constraint - it revealed to me the degree to which I typically rely on being able to pass around primitives.
The constraint drove our design to include a number of additional classes.
WE had a ```Cell``` class and a class to track their locations relative to one another (which we called ```World```), as might be expected.
We also ended up with a ```Location``` class and a ```Neighbors``` class, since we couldn't use such things as tuples, pairs, or collection-type datastructures to represent those constructs.

Overall, I had a great time and learned a lot. My experiences during the coderetreat have significantly reinforced my enthusiasm for pair programming - it is such a brilliant way to organically share knowledge between coders on a need-driven basis. The session constraints did a great job of pushing me out of my comfort zone and prompting me to try out some new strategies. And the group discussions after each session helped to reinforce what I had just learned and to pick up insights from other participants.

I look forward to my next opportunity to participate in a coderetreat.
