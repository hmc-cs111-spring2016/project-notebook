# Design notebook for week ending April 24, 2016

## Last week's critique

Didn't receive a critique last week :/

I did, however, get some solid advice from Prof Ben during class.  He told me about the difference between syntax and run-time errors and gave me some advice on using the syntax highlighter CodeMirror.

I also got some advice from William and Aaron on my GUI layout.  I asked them how I could have two different buttons for both tenor and alto clef, when their symbol looks the same.  They suggested I include the stave lines in the icon (they're placed differently on the stave), which I went ahead and did.

## Description

This week my time was split pretty evenly over three different tasks.
- Cleaning up my code and GUI layout.  Nothing too interesting here, but there are details in my work log below.
- Implementing basic syntax highlighting with CodeMirror.  This ended up taking a decent amount of time.  I used CodeMirror's [Simple Mode](http://codemirror.net/demo/simplemode.html) to define my highlighter in order to speed up the process, but it ended up being slightly less powerful than I would have liked.  For example, there's no way for me to define multiple "next" states for a given rule (this should make sense if you skim the documentation + example that I linked).
- Added more syntax errors to parser.  At this point I think I've covered most of the reasonable syntax error that could occur for the average user.  The code for my parser is starting to get bloated.  Not sure how I can better organize this (see below for more on organization issues).

Errors I currently support are as follows:
- Invalid parameter value
- Missing semicolon
- Missing parameter value
- Missing stave definition
- Missing closing curly brace
- Min range value greater than max value (this isn't actually a syntax error.  I'll eventually move it from parser and check at runtime).


#### Work Log

**Monday**: 2 hours  
-Critique for William

**Tuesday**: 1.5 hours  
-readded seeding (was removed while refactoring a while ago)  
-refactored javascript, code is now more properly segregated  
-GUI design work (redid clef buttons to differentiate alto, tenor)

**Wednesday**: 1.5 hours  
-GUI - max values smaller than min value automatically disabled (prevents integer-range errors from occuring) 

**Friday**: 1 hour  
-Started implementing code highlighting with CodeMirror

**Saturday**: 3 hours  
-Finished writing DSL "mode" for basic syntax highlighting  
-Added a bunch of errors

**Sunday**: 3 hours  
-Notebook entry  
-Prepared presentation

#### Basic Syntax Highlighting:

![highlighting](https://raw.githubusercontent.com/milohan/project-notebook/master/images/4-24_syntax_highlighting.png)


## Questions

**What is the most pressing issue for your project? What design decision do
you need to make, what implementation issue are you trying to solve, or how
are you evaluating your design and implementation?**

The [grammar](https://github.com/milohan/sheet-music-gen/blob/master/parserGrammar.js) for my parser is starting to get unwieldy.  Since I test and write my code in the [online version](http://pegjs.org/online) of PEG.js, there's no way for me to split the parser up into multiple files.  Even if I could, I don't know how I'd organize the different parts of the parser.  Currently my general structure is having "bigger" rules (eg Program, Stave) higher up, and smaller rules (IntegerRange, AlphaNumeric) further down.  Even with this general structure, it's starting to take longer and longer to find specific pieces of code, and it's also becoming more difficult to keep track of how the rules fit together.

Also, the simplest way I could come up with to handle certain types of errors was to make rules whose sole purpose is to return an error.  For example, see the "ImproperlyPlacedParameter" and "MissingValue" rules in the grammar code I linked above.  I'm not sure if there's a better method I could be using.

**What questions do you have for your critique partners? How can they best help
you?**

Do you have any suggestions on how I can organize my code for my parser, given that I'm writing/editing it using this [this](http://pegjs.org/online) web page? 

Is the method I'm sometimes using for error checking (creating "error" rules) bad practice?  In general, is there a universal way you're supposed to check for syntax errors when building a parser?  Currently I'm just doing whatever will work for each error.  It's working okay so far, but I'm worried about scalability.

**How much time did you spend on the project this week? If you're working in a
team, how did you share the labor?**

~12 hours