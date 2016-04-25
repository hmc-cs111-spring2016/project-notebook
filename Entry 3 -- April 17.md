# Design notebook for week ending April 17, 2016

## Last week's critique

Joshua did a lot this week to answer my questions the best he could. Below you will see a subsection for each question he addressed
and my comments on his thoughts.

###### Statements, expressions, and function calls
Interestingly enough, after thinking on it much this week, I think I will just make a container data type called Eval which simply
takes a call statement. This container would then call its corresponding function and, ultimately, replace itself in the Expr with
the return value. Additionally, interestingly enough, this also will help with type checking (if I get to that). For instance,
there could never be a void function in an Eval.

###### Function collisions
After dwelling on it, I agree that the `ship.` syntax is overkill. Additionally, I agree supporting overloading is more than
I probably want to deal with for now (although it might be interesting in the future)? That being said, true C doesn't support
this feature, so I don't really think I should bother. I am still not sure if I should add the prefix though (i.e.
`s_get_hull()` or `get_hull()`). I guess this would be a good question to bring up in class on Monday? 

###### Error checking vs. interpreter features
I think this should become an important discussion to have in class on Monday. While I agree quality error checking is
very important for the long term goals of this project, in the scope of the next few weeks, I need to really consider what
error checking aspects I want to support. A big one on my mind right now is type checking.

###### AST components for ship functions
I don't think I will bother to make custom AST components for the ship. The more I think about it, this just seems like it will
actually add more complexity to my interpreter than ease my work. Rather, I when interpreting a call function, I will first check
if the function name is included in a ship function name list, if it isn't I'll check for user defined functions.

###### Casting -- Big types -- Double / float / decimal /etc.
I am addressing all these comments at once. I think I am just going to support a traditional `int` and `double` for now. I could
always change this later. This is just the easiest to implement for now.

###### Arrays
I don't think I'm going to bother with arrays anymore. Too much of a headache for the scope of this projec.t

###### Order of operations
I think I may have gotten this all sorted out in my parsing! It might be difficult to add new operators in the future, but
for now I think this is good.

###### Assert
I think I am going to add `assert()`. It will just freeze the ship (not the whole game) and help the user debug. It actually
seems simple enough to implement too (the ship halting will be the same reaction to an error being caught).

## Description

This week I was super overwhelmend with clinic deadlines to want to tackle any of the more challenging parts of this project.
That being said, this didn't keep me from making progress this week. Thus, rather than working on the server for my project as
originally intended (i.e. the parser and interpreter), I chose to instead make progress on some of the more leisurely parts of
the software. Thus, this week, rather than directly addressing my dilemmas with my parser, interpreter, and language design
I chose to focused on thinking about how to address these problems while I coded some of the more trivial GUI components in
the client.

In addition to making progress on the GUI itself, I also began looking for sprites to use. Although I do have a friend
designing custom original sprites for me, it is seeming that they will not be done in time for this project. Thus, I have
located an alternative source of temporary sprites I can use for this project for free. This webpage, which can be found
[here](http://millionthvector.blogspot.com/p/free-sprites.html), provides users with high quality spaceship sprites
under the Creative Commons Attribution 4.0 International License. This means all I need to do for the time being is
credit the author for the sprites (thanks MillionthVector)! Ultimately, however, it is still my intention to replace these
sprites entirely with my own.

As for the parser and interpreter I plan to get them working completely this week (for real this time). To do this, I have
decided to add an extra type to Expr, an Eval which takes a Call type and executes it within the context of an Expr. This
way I keep the Call Statement and the Call Expr using similar data types but I can clearly extract and use a return value
when a function call is made in an Expr. As for the remaining progress I made on design with regard to the parser and interpreter,
this is addressed in the section above.

## Questions

**What is the most pressing issue for your project? What design decision do
you need to make, what implementation issue are you trying to solve, or how
are you evaluating your design and implementation?**
Interestingly enough, the most pressing issue for my project right now is time. Although I knew this week was going to be a struggle,
I was hoping to have completed what I considered to be the most challenging part of this project. That being said, I still believe
I am on track and will manage to get everything completed on time.

**What questions do you have for your critique partners? How can they best help
you?**
To be honest, since I didn't really make any progress on the most challenging parts of this project this week, I don't really have
many new questions at the moment. That being said, something I need to consider moving forward is the best way to test the game.

Therefore, how do you think I should test the game? Do you think I should just make a bunch of ship scripts that I run to
test for errors? Should I organize my critique group to play the current build of the game next week? Is there a testing framework
I should be using? I'm not entirely sure if using ScalaTest is really the best use of my time. Rather, might I be better off writing
scripts that I convert to AST and print to console so I can check the output by hand?

I firmly believe testing is a very important part of any code project, that being said, since so much of this game is how the user
interacts with it, I am not certain of how to best "automate" tests.

**How much time did you spend on the project this week? If you're working in a
team, how did you share the labor?**
This week, I spent the minimum 8hrs on this project thanks to clinic. That being said, since the most challenging deadlines have
now passed, I plan to spend a rediculuous amount of time on this project this upcoming week. I currently feel I am a little behind
at the moment.

