---
layout: post
section-type: post
title: The Liskov Substitution Principle
category: design
tags:
- SOLID principles
- design
---
The Lumpy Space Princ- ah, I'm sorry, the Liskov Substitution Principle (LSP) defines a relationship between substitutable types that aims to keep their shared behavior consistent. The LSP is one of the five SOLID principles. You can read about the other SOLID principles [here](http://scarvill91.github.io/tags/SOLID%20principles.html). Barbara Liskov dirst described the LSP in the following quote.

> "What is wanted here is something like the following substitution property: If for each object o1 of type S there is an object o2 of type T such that for all programs P defined in terms of T, the behavior of P is unchanged when o1 is substituted for o2 then S is a subtype of T." ([source](http://www.objectmentor.com/resources/articles/lsp.pdf))

For those like myself who aren't particularly fluent in mathspeak, a quote attributed to Uncle Bob puts it another way:

> "Functions that use pointers or references to base classes must be able to use objects of derived classes without knowing it." ([source](http://www.blackwasp.co.uk/lsp.aspx))

At its heart, the LSP is about using polymorphism well. In simple terms, the power of polymorphism lies in the ability to treat different things alike - that is, in the ability to substitute one type of thing for another. The downfall of violating the LSP is that this ability to treat different things alike falls apart. Your code will have to address the differences between types through all sorts of added type checking, variations of functions for each type, etc. But enough theory, let's try an example.

In a tic tac toe application, you'd likely have a class that handles the behavior of a gridded game board - which would primarily be recording the location of each player's moves. For the sake of this example, we decide to make this game board class abstract and to inherit from it to implement boards of varying dimensions (i.e. 3x3, 4x4, 5x5, etc). Among other methods not shown, our game board class has a method ```center_space_coordinates``` that returns the coordinates of the centermost space on the board.

<pre style="text-align: left">
class Board
  def center_space_coordinates
    raise error "#center_space not implemented in subclass"
  end
end
</pre>

This is simple to implement in our 3x3 board and our 5x5 board subclasses, but what of the even-sided boards that do not have an exact center - such as our 4x4 board?

<pre style="text-align: left">
# Note: the coordinate [0, 0] designates the upper-left space on a board

class 3x3Board < Board
  def center_space_coordinates
    [1, 1]
  end
end

class 4x4Board < Board
  def center_space_coordinates
    # ???
  end
end

class 5x5Board < Board
  def center_space_coordinates
    [2, 2]
  end
end
</pre>

To consider the consequences of whether we abide by the LSP in our implementation of even-sided boards, let's create a ```ComputerPlayer``` class that uses the ```center_space_coordinates``` as a simple optimization to selecting a move. If we returned ```nil``` in the case of even-sided boards, we would be violating the LSP because such boards would not be substitutable for odd-sided boards. Our ```ComputerPlayer``` class would not be able to treat different boards alike, like so:

<pre style="text-align: left">
class ComputerPlayer
  def initialize(board)
    @board = board
  end

  def select_move
    center_coordinates = @board.center_space_coordinates
    if center_coordinates != nil && @board.unmarked_at(center_coordinates)
      center_coordinates
    else
      # choose the coordinates of an unmarked space
    end
  end
end
</pre>

If we had even-sided boards return a collection of the 4 centermost spaces, this would also be a LSP violation. This version of ```ComputerPlayer``` chooses an available space from among the given center spaces if one is available.

<pre style="text-align: left">
class ComputerPlayer
  def initialize(board)
    @board = board
  end

  def select_move
    center_coordinates = @board.center_space_coordinates
    if center_coordinates.count == 4 && center_coordinates.any? { |coordinates| @board.unmarked_at(coordinates) }
      center_coordinates.each { |coordinates| return coordinates if @board.unmarked_at(coordinates) }
    elsif @board.unmarked_at(center_coordinates)
      center_coordinates
    else
      # choose the coordinates of an unmarked space
    end
  end
end
</pre>

Last, if we had even-sided boards return a random space from among their 4 centermost spaces, then we'd finally be abiding by the LSP - at least given the requirements of ```ComputerPlayer``` as it is now. This version can safely treat all boards alike, and is the simplest version of the three.

<pre style="text-align: left">
class ComputerPlayer
  def initialize(board)
    @board = board
  end

  def select_move
    center_coordinates = @board.center_space_coordinates
    if @board.unmarked_at(center_coordinates)
      center_coordinates
    else
      # choose the coordinates of an unmarked space
    end
  end
end
</pre>

Note my caveat, however. Our implementation of ```center_space_coordinates``` is still fudging things - and really, there's no good solution to this scenario. Despite any intuitive appeal our class hierarchy may have, the fact is that boards of different dimensions do not all have a single centermost space. There would be no way around this constraint if, for example, our ```ComputerPlayer``` assumed that there would be an equal number of spaces between the space supplied by ```center_space_coordinates``` and the edge of the board in every direction. This is a correct assumption about a true center space, but is guaranteed incorrect for any given space on an even-sided board.

This kind of scenario is what the LSP aims to prevent. You don't subtype something unless the types are truly substitutable for each other - in the sense that you can ignore their differences and treat them identically.