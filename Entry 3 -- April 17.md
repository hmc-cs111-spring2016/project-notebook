# Design notebook for week ending April 17, 2016

## Last week's critique

William's critique was extremely helpful in laying out a road map of features to implement.  Before talking to William, I had a few features in mind (rhythm generation, GUI version, syntax highlighting), but I wasn't sure how to prioritize them.  I was actually leaning towards rhythm generation, since it seemed to be the most interesting of the bunch.  William, however, suggested that I push it to the side for the time being, as it's more back-end heavy than anything else.  Instead he suggested I first work on the GUI version of the DSL and then focus on syntax highlighting/user feedback.  
I've decided to do just that (this week I focused mainly on the GUI version of the DSL), and I think my project is much better off for it.  I could've very easily fallen into a pattern of adding support for more and more backend-heavy parameters.  By instead focusing on newer, more language-y features, by the end of the semester I should have a decently-comprehensive vertical slice of my DSL.

William suggested some specific libraries/plugins I can use to define custom syntax highlighting for my DSL.  I haven't gone over them too in-depth yet, but from what I seem they're a much better option than my previous plan of writing a bunch of javascript to manipulate a textarea HTML element.

He also answered my questions about the intuitiveness of some specific parameter names, giving some suggestions that I might end up using.  He suggested that instead of going the camelCase route with "absRange," I should have users explicitly write "absolute range."  This makes sense at the moment, but I'll probably talk with some members of my target audience before I settle on a naming convention.

## Description

The majority of my time this week was spent designing and implementing the GUI version of my DSL (the code for which is viewable [here](https://github.com/milohan/sheet-music-gen/blob/master/dsl_ui.html)). In doing so I mostly stuck to my original plan of using GUI elements to mirror the original DSL in a one-to-one, but simplified, manner.  However, I did make two important design changes in the process.
- For the GUI version of the DSL, I've decided to get rid of default stave parameters.  The main reason for this change is to prevent confusion on the user's end.  The GUI version of the DSL is targeted at users who have no experience with coding, and possibly very little experience with technology in general.  To this audience, having a set of stave parameters that don't actually define a stave may not make much sense, and the ability to define default parameters doesn't actually save much time unless you're defining a huge number of similar staves (for such a case, I may eventually add a "Copy Stave" button).
- Decided to convert from GUI form elements to DSL code, instead of directly to internal representation of data.  Once I have the code, I can output sheet music using the same javascript code that I wrote for the original DSL.  This has two benefits:  
	- Because the GUI version mirrors the UI version in a one-to-one manner (i.e. for every parameter there is a corresponding form element) it's actually simpler to create lines of "code" than it is to create a Javascript object of arrays.
	- The ability to export DSL code means that users can start a program in the GUI version and then easily switch over to the code version at any time.  This is beneficial because the GUI version offers slightly less flexibility than the code version (for example, "polyphony" can only be a range of integers in the GUI version, but it can be specified as an integer, a range of integers, or a list of integers in code).

#### GUI Version of DSL:
![](https://raw.githubusercontent.com/milohan/project-notebook/master/images/4-17_gui.png)

#### Code output:
![](https://raw.githubusercontent.com/milohan/project-notebook/master/images/4-17_gui_code_output.png)

**Monday**: 1.5 hours  
-Started critique for Aaron  
-Refactoring (started getting rid of unnecessary reorganization of data in "parse" method).

**Tuesday**: 1 hour  
-Finished critique for Aaron

**Wednesday**: 1 hour  
-Refactoring

**Friday**: 1 hour  
-Designed and started implemenitng GUI version of DSL

**Saturday**: 4.5 hours  
-Worked on frontend for GUI version of DSL  
-Started conversion from form elements to DSL code

**Sunday**: 2.5 hours  
-Wrote notebook entry
-Worked on GUI DSL - now outputs sheet music

## Questions

**What is the most pressing issue for your project? What design decision do
you need to make, what implementation issue are you trying to solve, or how
are you evaluating your design and implementation?**

I'm currently trying to figure out how I'll provide error messages/feedback for the GUI version of my DSL.  Currently, the only way users can create an invalid program is by doing one or more of the following:
- Leaving the "measures" field blank
- Entering a negative value in the "measures" field (the field is number-only, so I don't have to worry about other invalid types of input)
- Selecting a max "polyphony" from the dropdown menu that's lower than the min "polyphony".

As feedback, I could simply return the same parse/logic errors I'd return in the code version of my DSL.  However, a better method would probably be to prevent users from clicking the "generate" button until they have a valid program.  In the case of the "polyphony" error, I could prevent it from ever occurring by blanking out max "polyphony" values that are lower than min "polyphony values" (and min values that are greater than max values).  For the "measure" errors I could display some sort of warning when users enter something invalid.

**What questions do you have for your critique partners? How can they best help
you?**

Questions on specific form elements:
- For the "clef" form element: There are actually 4 clefs that I support in the code version of my DSL (treble, bass, alto, tenor), but the alto and tenor clefs look exactly the same (one is simply placed higher in the stave than the other).  Since I'm using images of the clefs in my form, it would be confusing to have both alto and tenor, so I left one out.  Is there any way you can think of to include both without causing confusion?  I was thinking of just adding a label, but that would still look kind of weird imo.
- How should I make the "range" element clearer?  I like the current setup of the double slider, but there isn't any feedback yet of what notes the user is actually setting the min/max to.  I'm planning on adding a label for the current min/max note in scientific pitch notation (eg "C4"), but that still doesn't mean much to many instrumentalists.  Maybe add a mini cutout of a score with the min note and max note on it?  Or is that overkill?

What do you think of the direction of my GUI so far?  Any elements other than the two I listed above that could use some reworking?

**How much time did you spend on the project this week? If you're working in a
team, how did you share the labor?**

Spent 11-12 hours on the project this week.