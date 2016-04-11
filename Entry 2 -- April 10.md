# Design notebook for week ending April 10, 2016

## Last week's critique

**TODO:** Fill in this part with a summary and reflection on the critque you
received for last week's work. Answer questions such as:  How, specifically, did
the feedback help improve the project? Did the feedback point out or offer
something you hadn't considered? Did it help you make a design decision? Was it
helpful in addressing the most pressing issues in your project? How will you
incorporate the feedback into your work? Will you change something about the
design, implementation, or evaluation as a result?

The critique I recieved (as well as talking to my critique group and Prof. Ben in class) was very helpful and I'd say it shaped the approach I took for my prototype this week. The feedback specifically helped me decide what to implement first, and generally speaking what to focus on for the remainder of the course.

## Description

**TODO:** Fill in this part with information about your work this week:
important design decisions, changes to previous decisions, open questions,
exciting milestones, preliminary results, etc. Feel free to include images
(e.g., a sketch of the design or a screenshot of a running program), links to
code, and any other resources that you think will help clearly convey your
design process.

I have a parser and intermediate representation! It's nowhere near complete, but you can already put in all reasonable durations (not counting triplets and whatever) and frets. It's possible to change durations basically wherever in a sequence of notes. Unlike VexTab, which uses slightly different syntax if you change duration at the beginning of a sequence of notes as opposed to somewhere in the middle, my language has exactly the same syntax regardless, because as far as I'm
concerned that just makes more sense. Have a look at my project code. I need to organize the whole thing better, but you can find test.tc, test.ly, and test.py in the src folder in my tab-checker repository. test.py is the current glue holding my parser and intermediate representation together, and it takes in whatever text is in test.tc, parses it into my IR, and outputs lilypond to test.ly. The test.ly that's currently in the src folder was created from the test.tc that's
currently there! I've compiled it and it does exactly what it should! For convenience, the tab-checker repository is [here](https://github.com/Arc11295/tab-checker).

## Questions

**What is the most pressing issue for your project? What design decision do
you need to make, what implementation issue are you trying to solve, or how
are you evaluating your design and implementation?**

I think what I need to do this week is pick a few more features to add to the language (that is, to the parser and intermediate representation), and also make the interface between them more modular. I have a rudimentary intermediate representation and parser already working. The thing is, the way I've stitched them together for now is, in my opinion, kind of janky and certainly not terribly well-suited to expansion (although the parser and intermediate representation are, for the
most part). That's probably because I threw that part together in like an hour after finishing the first iteration of the parser. It's possible that I should use a different parsing library so I'm not constructing an AST and then walking it to create my intermediate representation, although my IR is only vaguely tree-structured so I don't know how I'd create it _while_ parsing, either.

**What questions do you have for your critique partners? How can they best help
you?**

I chose to use numbers for durations because I found that a little easier to code, but now that I've had a chance to sleep on it I'm finding that it's kind of hard to tell at a glance what's a duration and what's a fret. I'd like to know what others think about that--I imagine it'd be easier to read and not really harder to write if I used letters instead (e.g. "w" for whole, "h" for half, "q" for quarter, "e" for eighth) rather than my current system _or_ VexTab's half-letter
half-number why-the-heck-did-they-do-it-this-way system.

On the topic of language features, what might be the next-most-important features? In other words, what should I add next?

I'd also appreciate suggestions about how to better put together my parser and intermediate representation.

**How much time did you spend on the project this week? If you're working in a
team, how did you share the labor?**

I forgot to keep a log of it but counting this write up I estimate I spent between 9 and 12 hours on the project.
