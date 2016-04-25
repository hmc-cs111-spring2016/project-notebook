# Design notebook for week ending April 24, 2016

## Last week's critique
After reading through the critique, I feel I have addressed most of the issues (since there wasn't many at this phase of
the project). That being said, as I'm winding down the project, I am sad to realize a lot of the feature I was hoping I'd get
to I simply won't have time for. That being said, I am really proud of what I accomplished since it is far from a simple task.

#### Simplifying Function Calls
Although I am not sure I would call the Eval container a _simplification_ of the grammar, I feel confident this was the
correct way to address the issue at hand. This way I can still rely on the parsing for Statement Function Calls while
still being able to evaluate them within the context of a Expr.

A big hold up I have come to realize is it is impossible to really keep Statements and Expr completely separate for C-style
language (in the Programming Languages definition). Functions in a technical definition should not cause side effects, but
that is clearly not the case for C-style languages.

#### Bulid Environment
Although I'm pretty sure it all works now, I actually caught a pretty big problem with this this week. Interestingly enough
although IntelliJ can import and run Gradle projects and project structures, after you start making changes inside IntelliJ,
the changes are only reflected in the IntelliJ project (as far as I've discovered... I wouldn't be surprised if there is a
way to make IntelliJ generate Gradle build files though). Because of this, even though the code would build and run in
IntelliJ, the code would not actually run with `./gradlew run` as it should. I fixed this by adding 
`compile 'org.scala-lang:scala-parser-combinators:2.11.0-M4'` to the build file. This fetchs important things like PackratParser
from the online Maven repo. (To be honest, I'm surprised this isn't included in the Scala core library...)

#### Testing
Although still not as rigorous as I was hoping, I began writing simple scripts that I could run through my parser to help
me debug various parser problems. The most important of these is actually the third program called `prog3.bs`. This is the
sample program I wrote for my prototype and is the most complicated test at the moment.

For the time being, I just pushed them in a root server directory. Although I know this is horrible style, this was the
easiest directory to place the files in for now. When the program runs, this is the directory that is used
as the working directory. This means I can avoid having to place absolute paths in the code in favor of just file names.
Although I plan to address this before Friday (I plan to make a dedicated and documented sample script folder).

## Description

#### Finalizing Parser
I got the biggest achievement of this entire project working this week - the parser. Although I definently want to do more
with it, espeically in the realm of error and type checking, it is in a great place. As it stands now, I can put in a full
"complex program" and parse it correctly. Although I will refrain from linking `prog3.bs` here (since I imagine the 
filename/location will change in the near future), the types of programs it can handle is almost identical to C barring
the preprocessor, arrays, structs, and pointers (which although is a lot, isn't necessary for my purposes). Take a look in
the repo for sample programs for a better understanding of what it can parse successfully.

In order to get a lot of this stuff working, I needed to make my parser a lot more complex. What used to be a single funciton
`pStatement` is now 6 different functions: `pStatement`, `pStatementBlock`, `pStatementSemicolon`, `pStatementControlFlow`,
`pStatementDecl`, and `pStatementAssign`. Although I am quite happy with the current break down of these individual parsers,
I am not very confident in my naming convention. As it stands, I feel these names imply they each parse ONLY the specific type of
statement in the name, when in reality they call each other in order (i.e. `pStatement` => `pStatementBlock` => `pStatementSemicolon`
=> `pStatementControlFlow` => `pStatementDecl` => `pStatementAssign`). I'm not really sure how to address this.

I am also happy to report I even managed to implemented a few extra operators I was originally teeter tottering on implementing
(i.e. `++`, `--`, `&&`, and `||`). That being said, these expressions do not necessarily interact correctly with each other
at the moment. As seen [here](http://en.cppreference.com/w/c/language/operator_precedence), C operator precedence is well
define such that things like `||` should be evaluated after `&&`. Thus, since I implemented all the boolean operators at one
level in the parser, they just parse out in order from left to right. Although at this point I know exactly how to fix this,
I decided to leave it be in favor of a few other tasks.

Note: Something huge that tripped me up this week is the difference between `()` and `{}` for function declrations in Scala!
Specifically, I was getting confused when the functions wouldn't allow me to put the `|` operator on the next line.

For instance, the following compiles correctly:

    def pType: PackratParser[Type] = (
            "int"       ^^^ IntTy()
        |   "double"    ^^^ DoubleTy()
        |   "bool"      ^^^ BoolTy()
        |   "char"      ^^^ CharTy()
        |   "void"      ^^^ VoidTy()
    )

But this (I only switched the `()` with `{}`) does not compile correctly:

    def pType: PackratParser[Type] = {
            "int"       ^^^ IntTy()
        |   "double"    ^^^ DoubleTy()
        |   "bool"      ^^^ BoolTy()
        |   "char"      ^^^ CharTy()
        |   "void"      ^^^ VoidTy()
    }
    
But then this (I put the `|` at the end of the previous line instead of the beginning of the next) compiles correctly:

    def pType: PackratParser[Type] = {
        "int"       ^^^ IntTy()     |
        "double"    ^^^ DoubleTy()  |
        "bool"      ^^^ BoolTy()    |
        "char"      ^^^ CharTy()    |
        "void"      ^^^ VoidTy()
    )

When considering what is actually happening, this actually makes a lot of sense. When using `{}`, this implies to Scala the
code block is "more than one line". Thus, if you don't end the line with a binary operator, it can't determine if the line
is supposed to end or not. Python does something similar where you can't start a line with a `+` unless you ended the previous
line with a `\` (telling Python to continue to the next line) but you can end a line with a `+` and it will continue the expression
onto the next line without the `\`.

When you use `()`, however, this implies the entire block (if you can even call it that) is just one statement. This means you
can start the line with a binary operator since Scala knows the whole thing is a single expression.

I also notice while I was programming I started using the `return` statement again, something you don't often need in Scala 
(since it implicitly returns the last value in a function). There is probably quite a bit I could do to make my code more
Scala-like in style. I am jumping a lot between thinking about C and Scala (due to the nature of the project) keep catching
myself reverting to non-Scala styles. Also, I just am not always certain of the most Scala-like way to express something.
I would benefit greatly from a code review from a more experience Scala programmer like Prof Ben.

#### Building Game Logic
Although I started a majority of this work last week, I ended up revamping portions of it as I began working on the interpreter
and network communication. As I continue to mull over future features I wish to implement (i.e. multiple weapons, armor types, etc.) 
I believe the complex granularity I chose to work with makes a lot of sense. That being said, I'm still certain this current
formation isn't final. You can check out the majority of the current implementation by checking out `Behaviors.scala` and `Draw.scala`.

As of now, the traits I have are:

   * `Acceleration`: A trait that inherits from `Velocity`. Possessing this trait means movement depends on velocity and acceleration.
   * `Allegiance`: A trait that inherits from `Solid`. Possessing this trait means the object has an alliance with other units. This means its collision behavior only affects enemies. This is useful for controlling friendly fire.
   * `Corporeal`: A trait that most things inherits from (last week this behavior was a part of `Drawable`, now `Drawable` possesses this trait). Possessing this trait means the object has a position and direction in the "world." I'm not sure how I feel about the name.
   * `Expiration`: A trait that inherits from `Mortal`. Possessing this trait means the object will die after a certain duration regardless of the circumstances.
   * `Explosive`: A trait that inherits from `Expiration` and `Solid`. Possessing this trait means the object will die upon collision with something and inflict damage.
   * `FFExplosive`: A trait that inherits from `Explosive` and `Allegiance`. Possessing this trait means the object will only explode and damage enemy units. The FF stands for Friendly Fire.
   * `Health`: A trait that inherits from `Mortal`. Possessing this trait means the object has health and will die when it reaches or goes below 0.
   * `Mortal`: A trait that means the object can die. This can be useful for differentiating between background indestructable objects and player objects.
   * `Movable`: A trait that means the object has an `updatePos()` function. This means the object can move between game loops.
   * `Solid`: A trait that inherits from `Corporeal`. Possessing this trait means the object can collide with other `Solid` objects.
   * `Velocity`: A trait that inherits from `Movable`. Possessing this trait means movement depends on velocity.
   * `Drawable`: Not a trait, but an important Abstract Class. Anything that inherits from this can be drawn on the client side display. Drawable uses the `Corporeal` trait since a drawn object needs a location on the map.

#### Network Logic
I decided to simplify my client for the time being by making it only a viewer. This means the server does not need to deal with
receiving any of the information from the client. Thus, the client basically just draws on screen the information sent to it by
the server from the `Drawer` and `DrawQueue`. To do this, I strip down every `enabled()` object that inherits from the `Drawable`
abstract class into a `DrawOrder` and send serialize it over the socket. I should really consider changing my network connections
to UDP (to prevent lag over a bad connection), but I don't want to deal with missing packets. Regardless, advancements over the
networking is a much lower priority now.

#### Building Interpreter
One of the biggest things I have not fully got working this week is the interpreter. Specifically, I am unsure of how to overcome
a typing problem in Scala although I am certain it has to do with Scala Reflection. I will outline this more in the question section.

## Questions

#### What is the most pressing issue for your project? What design decision do you need to make, what implementation issue are you trying to solve, or how are you evaluating your design and implementation?

Although it is clear I will not get everything I hoped to get done, done, I would like to get the interpreter working a bit more
than it currently is. Although at the moment I am fine with dealing with types within my code, I am at a loss of how to convert
one of my types elegantly to one of Scala's types for implementation. Furthermore, when building my function `Map` environment, since
my various functions have different parameters and return types, I can't simply make a `Map[String, () => Unit]`.

One solution I've considered is making a psudo-stack system where arguments are sent and values are returned in shared global
variables. That being said, there is still some typing problems one runs into down this path too. Specifically, if I have a
function that simply does pattern matching and returns the Scala equivilent to my type, this means the Scala function must
return something of type `Any` or `AnyValCompanion`.

I've been spoiled a bit from Python which can just arbitrarily return any type as it sees fit.  I know this is a problem Scala
can address and I know the solution lies in Scala Reflection somewhere, but I'm having trouble figuring it out.

#### What questions do you have for your critique partners? How can they best help you?

###### What's a good place to stop?
At this point, I will obviously not get everything I'd like to get done, done. The real question is where should I stop?
Especially after this last week, I've certainly put much more time than I was required to into this project. I'm just curious
where I should call it quits.

As it stands, a lot of individual parts work but the whole system cannot run as a whole. Should I focus on getting the whole
thing running in some limited capacity? Or should I focus on having a runnable demo for each of the individual parts?

###### Naming Subparsers
What do you think of my subparser names? They got a lot more haphazard as I began making some big changes to the parsing levels
of my program. I'm not really sure what a good convention is. Any ideas?

###### Trait Inheritance
Although it is probably hard to comment on whether my trait inheritance makes sense without knowing where I am planning to go
with this project beyond the scope of this class, I do know going "inheritance crazy" (i.e. making way too many 
classes/traits/interfaces) is a problem programmers need to watch out for. Do you think this granularity makes sense?

###### Scala Reflection
Although I have some experience working with Java Reflection, Scala Reflection seems quite a bit different. I've also never
really tried to do anything this complicated with runtime typing in a strongly type language (i.e. Python's indifference makes
this stuff much easier). I looked at some tutorials, but I'm not even certain of the "name" of what I'm trying to do is. Any
ideas of how to figure this out? I'm planning on chatting with Prof Ben.

#### How much time did you spend on the project this week?
I spent a ton of time working on this project this week. Since I didn't have an Algorithms Homework (since we had a midterm this
week) I managed to spend over 20+ hours on this project. A lot of this was spent coding, debugging, and planning out inheritance/parser levels.

I also spent 3 hours planning, getting pictures, and practicing my presentation. (Some of the pictures took a while to make...)


