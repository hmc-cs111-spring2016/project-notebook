# Design notebook for week ending April 10, 2016

## Last week's critique

Overall, Josh doesn't seem to have any major objections to the current state or direction of my
project. It seems his biggest fear at the moment is getting all the individual parts working
together (which is a very realistic fear to have). That being said, I am confident I will be
able to complete what needs to be done. In many ways, I believe the most challenging part of this
entire project is getting the parser to work. Although I am still yet to get it completely working,
I have made great progress on this during the last week and plan to have it complete by next week.
I have also completed the AST baring one issue (that I will address later).

###### How, specifically, did the feedback help improve the project?
Josh did a good job of addressing my questions from last week.

I am happy he seems to think my scheme for declaring and naming globals is good. Although I might
reconsider my approach later (I think I might prefer the declaration go in the global scope and
then be initialized within `void init()`) his confirmation of my current plan makes me happy enough
to just make this decision for now and move on.

I also agree that just committing my assets to the repo in an `/asset/` folder will be best for now.
Although I might not want to push a huge amount of images and sounds to my GitHub repo later, I doubt
the assets will be too overwhelming before the end of this project.

There are a few answers I'm not sure if I entirely agree with, however.

After considering it, one suggestion, replacing the word `void` with a clearer word, I think defeats
target goal of this language in some ways. While I agree `void` isn't the clearest word to describe
a return type, I think the teaching philosophy of this game currently relies heavily on a teaching
methodology sometimes called the *Heuristic Method*. As I've always understood it (and as it seems
to be described in this
[link](http://www.studylecturenotes.com/curriculum-instructions/heuristic-method-of-teaching-meaning-advantages-disadvantages)),
the *Heuristic Method* revolves heavily on discovery and developing personal problem solving skills
with lots of individual feedback. By this thought, my goal isn't to "dumb down" the language, but to
simply present it in such a way that promotes exploration and good feedback. While I agree substituting
the word `void` isn't the worst offense of "dumbing down" the language, `void` is a very widely used
return type in programming world (i.e. C, C++, C#, Java, Scala, Javascript, etc all have the word in
some capacity). Thus I think this a good word to expose the users to. That being said, I am considering
changing the name of the `double` type (since I don't support `float`) for this type of reasoning. I
will explore this more later.

Additionally, while I agree the `.` operator is useful, I'm still not certain if I want to be using it
to call functions on the ship. Although subtle, `get_vel_max()` verses `ship.get_vel_max()` implies object
oriented programming, something I've otherwise mostly been able to avoid. I do agree the `.` operator
will be useful for access structs, however, if they make it into the language.

###### Did the feedback point out or offer something you hadn't considered? Did it help you make a design decision?
Josh is correct in saying I am eventually going to have to make a choice between adding advance features and advance
error checking. While I plan to have both in some degree, I could spend months on either of these tasks independently.
Thus, he is correct in pointing out this upcoming decision.

For the time being, I am planning on implementing enough features that my updated prototype sample program can be
run. Additionally, I wish to minimially support all the listed ship API function calls I provided in this document.
I would also like to add type checking for evaluation.

After this, if I have more time, I will make a decision to either add more implmementation or more error checking.
That being said, adding more implementation would probably be easier since it doesn't involve touching the parser
as much (or possibly at all).

###### Was it helpful in addressing the most pressing issues in your project?
Josh did provide a useful suggestion for resolving how to parse good error messages. Rather than looking for a 
parser with better error handling, I could just anticipate the most common errors and parse for them. While this
might seem like a big task, it is already something I was planning on doing in many ways since I intend to record
voice lines for a multitute for errors (you need to anticipate the errors if you want to record voice lines for them!)

###### How will you incorporate the feedback into your work?
The most immediate change I am incorrperating was something we discussed in person in class. Rather than referring
to ship API funcitons in camelcase, I am instead using underscores. Although C authors have been known to use both
of these conventions, I think `_` is more widely used (or at least looks more C-like to us).

###### Will you change something about the design, implementation, or evaluation as a result?
I did! I am now using names like `get_vel_max()` instead of `getVelMax()` for ship level API calls. Additionally,
I will probably parse for specific errors as part of my error checking. Finally, although I am differing the
decision, I will have to decide whether to prioritize more error checking or features at some point.



## Description

###### Important Design Decisions
   * Design the AST
   * Start designing the parser
   * Begin forumlating the interpreter and how it will interact with the AST
   * Looked at Prof Ben's Garden Project a bit after the fact (didn't want to take the fun out of thinking through the design challenges). This project can bee seen [here](https://github.com/hmc-cs111-fall2015/garden).
   * Although I can attempt to walk through these decisions with words, I believe the supplied code below will do a better job.

###### Changes to Previous Decisions
   * Changes API functions to use `_` instead of camel case (i.e. `void getVelMax()` is now `void get_vel_max()`)
   * No other major changes were made to last week's decisions. Most changes will like start happening when I begin integrating the individual parts

###### Open Questions
   * I have many open questions at the moment. See the questions section at the bottom for more details.

###### Exciting Milestones
   * Completed the AST (barring one open question)!

###### Preliminary Results
   * AST compiles fine
   * Since the Parser is not yet complete and the interpreter hasn't been started, I haven't been able to gather any actual results yet (next week)

#### Updated Prototype Program
Below is an updated version of the program I formulated last week. The main changes here are bug fixes and updating the naming
convention of the ship API functions. I used this program as a skeleton when designing the AST. Anything I express here needs
to be representable by the AST. I still need to figure out how to support casting so `get_hull() / get_hull_max()` doens't use
integer division.

    void init() {
      int g_flee = 0;
      set_vel(get_vel_max());
    }
  
    void main() {
      double percent = get_hull() / get_hull_max();
      if (percent > .2) {
        aggressive();
      } else {
        flee()
      }
    }
    
    void aggressive() {
      if (radar(0) > 0 && can_fire()) {
        fire();
      }
    }
    
    void flee() {
      int i = 0;
      for (i = -15; i <= 15; i++) {
        if (radar(i) > 0) {
          g_flee = i;
          break;
        }
      }
      if (g_flee != 0) {
        int turn;
        if (g_flee > get_rot_max()) {
          set_rot(get_rot_max());
          g_flee -= get_rot_max();
        } else if (g_flee < -get_rot_max()) {
          set_rot(-get_rot_max());
          g_flee += get_rot_max();
        } else {
          set_rot(g_flee);
          g_flee = 0;
        }
    }

#### Prototype Ship API Calls
Below is the initial ship API functions I plan to support. In the future, different ships will have different components
(i.e. different weapons, engines, etc.) which will change which subset of these functions a ship has access to. This "hardware"
is designed to be as simple as possible.

In the future I would like to make more complex weapons, movement, etc. For instance, a weapon with ammo and different
firing patters would be interesting. Additionally, having engines work based of acceleration, instead of velocity, make
control much harder (and thus interesting).

##### Weapons -- Basic Blaster
   * `void fire()`: If the basic blaster can fire, it fires. If it cannot fire, it throws an error.
   * `bool can_fire()`: Returns `true` if the basic blaster can fire and `false` otherwise.

##### Engine -- Movement
   * `void set_vel(int v)`: Sets the ship's velocity magnitute to the provided number. If the number is greater than the maximum velocity it throws an error.
   * `int get_vel()`: Returns the ship's current velocity magnitute.
   * `int get_vel_max()`: Returns the maximum ship velocity magnitute.
   * `void set_rot(int r)`: Sets the ship's rotation velocity in degrees/update. In otherwords, `r = 0` means directly forward, `r < 0` means some degrees off in the clockwise direction, and `r > 0` means some degrees off in the counter-clockwise direction.
   * `int get_rot()`: Returns the ship's current rotation velocity.
   * `int max_rot()`: Returns the maximum ship rotation velocity.

##### Hull -- Health
   * `int get_hull()`: Returns the ship's current health.
   * `int get_hull_max()`: Returns the ship's maximum health.

##### Sensor -- Detection
   * `int radar(int angle)`: Looks in the provided relative direction. If there is an object that is within the radar's maximum range, it will return the distance. Otherwise it will return `-1`.
   * `int radar_range_max()`: Returns the maximum range the radar can detect objects.

##### Communication Link -- Debugging
   * `void alert(String msg)`: Displays a string message above the ship in game. Useful for debugging.

#### Prototype AST
Although it is a lot of code, I think discussing the curerent state of the AST is very important for my prototype.
Especially since I am not sure how to parse Call (i.e. it is both an expression and statement).

For the prototype demo, it might be interesting to try to build the AST for the sample program (especially the `void flee()`
function).

      case class FuncDecl(rtnTy: Type, name: String, param: List[VarDecl], body: Block)
      
      case class Type() // TODO add ArrayTy
      case class IntTy()        extends Type
      case class DoubleTy()     extends Type
      case class BoolTy()       extends Type
      case class CharTy()       extends Type
      case class VoidTy()       extends Type
      
      case class VarDecl(ty: Type, name: String)
      
      case class Expr()
      case class Literal()                        extends Expr // TODO add ArrayLit
      case class IntLit(i: Int)                   extends Literal
      case class DoubleLit(d: Double)             extends Literal
      case class BoolLit(b: Boolean)              extends Literal
      case class CharLit(ch: Char)                extends Literal
      case class Var(name: String)                extends Expr
      
      // uniop
      case class NegateOp(expr: Expr)             extends Expr
      case class NotOp(expr: Expr)                extends Expr
      
      // binop
      case class AddOp(left: Expr, right: Expr)   extends Expr
      case class SubOp(left: Expr, right: Expr)   extends Expr
      case class MultOp(left: Expr, right: Expr)  extends Expr
      case class DivOp(left: Expr, right: Expr)   extends Expr
      case class ModOp(left: Expr, right: Expr)   extends Expr
      case class AndOp(left: Expr, right: Expr)   extends Expr
      case class OrOp(left: Expr, right: Expr)    extends Expr
      case class LtOp(left: Expr, right: Expr)    extends Expr
      case class GtOp(left: Expr, right: Expr)    extends Expr
      case class LeqOp(left: Expr, right: Expr)   extends Expr
      case class GeqOp(left: Expr, right: Expr)   extends Expr
      case class EqOp(left: Expr, right: Expr)    extends Expr
      case class NeqOp(left: Expr, right: Expr)   extends Expr
      
      case class SubscriptOp(left: Expr, right: Expr)  extends Expr
      
      case class Statement()
      case class Block(body: List[Statement])                                                     extends Statement
      case class IfThenElse(cond: Expr, bodyTrue: Block, bodyFalse: Block)                        extends Statement
      case class Call(name: String, args: List[Expr])                                             extends Statement
      case class For(init: Statement, cond: Expr, inc: Statement)                                 extends Statement
      case class While(cond: Expr)                                                                extends Statement
      case class Break()                                                                          extends Statement
      
      // side effect op
      case class Assign(name: String, expr: Expr)                                                 extends Statement
      case class DeclAssign(decl: VarDecl, expr: Expr)                                            extends Statement
      case class AddAssign(name: String, expr: Expr)                                              extends Statement
      case class SubAssign(name: String, expr: Expr)                                              extends Statement
      case class MultAssign(name: String, expr: Expr)                                             extends Statement
      case class DivAssign(name: String, expr: Expr)                                              extends Statement
      case class ModAssign(name: String, expr: Expr)                                              extends Statement
      case class PreIncOp(expr: Expr)                                                             extends Statement
      case class PreDecOp(expr: Expr)                                                             extends Statement
      case class PostIncOp(expr: Expr)                                                            extends Statement
      case class PostDecOp(expr: Expr)                                                            extends Statement



## Questions

**What is the most pressing issue for your project? What design decision do
you need to make, what implementation issue are you trying to solve, or how
are you evaluating your design and implementation?**

Before next week I really need to completely finish the parser and AST for
basic programs (without error checking). I really want to be able to generate
AST from simple correct programs.

This means I am going to need to figure out how to represent function calls.
I ran into a little problem I haven't been able to address yet when I realized
function calls can behave as both statements and expressions.

Additionally I still need to figure out how I want to handle casting.

**What questions do you have for your critique partners? How can they best help
you?**

   * Statement are usually considered to have side effects while Expressions do not. That being said, this seems to only be a formal definition. For instance, things like `break` are merely control flow and thus has no side effects (although one could make the argument by breaking a loop out of scope it is a side effect?), but as far as the parser is concerned, `break` is definently a statement. This is especially an issue for `Call(name: String, args: List[Expr])`. A `Call` can definetly be a statement but can also be evaluated in an expression. This problem seems to be rooted in addressing order of operations in expressions, something I have done mathematically in the parser but have not done for all operators (since some, like `+=`, have side effects and others, like arithmetic `+`, do not. How should I handle and parse `Call` in the AST?
   * Should there be function collisions with ship API calls? Or should I call all ship API functions with either `ship.` or some prefix like I'm using for global variables (i.e. `g_`)?
   * After basic features are supported, should I prioritize more error checking or more interpreter features?
   * Should ship API functions get their own AST components since they directly cause game behavior and are predefined?
   * If ship API calls should get their own AST components, should the parser parse them out directly or should I preform a transformation on `Call` items during type checking to convert them into the appropriate AST items.
   * How should I support casting? I am uncertain at the moment of how to support the C parenthesis method (i.e. `(int) 3.5`). Would it be better to support it with a few predefined functions like `int2double`, `double2int`, `int2char`, etc.? Does the latter betray my teaching methodology?
   * Should I support treu `int` operations or should I wrap them in a BigInteger type? If I do this, I can prevent overflow errors.
   * Should I support true `double` operations or should I wrap them in a BigDecimal type? If I do this, I can keep a fixed decimal point percision (i.e. 6 decimal place values) and avoid weird rounding issues.
   * Since I am not supporting `float` type, does calling the decimal type `double` make sense? Should I just call my `double` `float` or should I call it `decimal`?
   * Should I bother supporting arrays? For the time being, I think this is a huge layer of unneeded complexity.
   * If I choose to support Arrays, it seems obvious `String` will be some kind of wrapper around `Array[CharTy]`. That being said, if I choose not to bother with arrays, how much should I handle `String`? I am considering supporting them only as constants so you can developers can create useful error messages for themselves but not allowing much (any?) String manipulation.
   * How much do I deal with order of operations? That seems to be the root of my `Call()` statement problem.
   * Should I support `assert(bool b)`? I think this is extremely useful for users.

**How much time did you spend on the project this week? If you're working in a
team, how did you share the labor?**

I spent a solid 8.5 hours working on this project outside of class this week. I would have
liked to spend enough time so I could have ensured the AST and parser were complete this week,
but clinic prevented me from doing extra this week (Phase III presentation and a draft of
our final report is due next week!)
