# Design notebook for week ending April 3, 2016

## Last week's critique

**TODO:** Fill in this part with a summary and reflection on the critque you
received for last week's work. Answer questions such as:  How, specifically, did
the feedback help improve the project? Did the feedback point out or offer
something you hadn't considered? Did it help you make a design decision? Was it
helpful in addressing the most pressing issues in your project? How will you
incorporate the feedback into your work? Will you change something about the
design, implementation, or evaluation as a result?

The critique from last week was very helpful in shaping the direction of the project. I still believe that my DSL can be useful for checking tabs, and I also believe that when creating a tab of one's own a GUI-based editor is actually easier for iterating and improving, based on my own experience. However, I also now recognize that perhaps the best use-case for my DSL will be creating the first version of a completely new tab. While my language won't have some of the features that make GUI editors nice for modifying
tabs, it will still be a lot faster to use for inputting a whole song. The critique also pointed out VexTab, which does basically the same thing as I originally wanted my language to do. For these reasons I have decided to make it a much higher priority to include TuxGuitar-readable output. I will still focus on the PDF/MIDI output since LilyPond is better documented than the TuxGuitar utility library (see my design and implementation overview in my project repository for how
I'll be using these) and I won't have to fight with the back-end as much that way, but if there's a week left and I haven't even started with TuxGuitar output, I'll now spend the last week on that. As for the front-end, VexTab is syntactically similar to what I've had in mind since coming up with the project, so I will use it as inspiration; there are some things that I'll probably copy nearly verbatim (VexTab does tuplets really nicely, for one thing), and others that I really don't
agree with (the syntax for bending notes doesn't make that much sense to me and also corresponds to a pretty weird output).

Since I didn't come up with that detailed of an evalutation scheme last week, I'll try to do a bit better here. My language will perform satisfactorily if it meets the following requirements. Notes should be able to have durations that are anything from whole notes down to 64th notes. Dotted notes are also allowed. It's okay if it only supports regular six-string guitars by the end of
the class, but it should be possible to specify what pitch each string is tuned to. There doesn't strictly need to be a cap on what fret numbers are allowed, but I need to at least have a plan for how I would deal with an input indicating the 42nd fret on the 6th string, which is patently ridiculous. Rests need to be implemented as well (it's probably ok for them to be a special case of regular notes). Multiple tracks and multiple voices on the same track don't need to be done by the end of the course, but I absolutely cannot make any
design choices that make these impossible to add. Chords should be implemented. 4/4 and 3/4 time signatures should be available, but others don't have to be. Everything that's parsable must also transpile to LilyPond, and at least time signature and note durations/frets should be outputtable to TuxGuitar. For the sake of my sanity, I may make the "too much duration in one measure" error not an error as follows: try to fill a measure as normal, but if a note is too long to fit in the
remainder of the measure, move it to the next measure and fill the end of this measure with rests. In this case, any notes that are too long to fit in an empty measure (e.g. a whole note in 3/4 time) will just be ignored.

## Description

**TODO:** Fill in this part with information about your work this week:
important design decisions, changes to previous decisions, open questions,
exciting milestones, preliminary results, etc. Feel free to include images
(e.g., a sketch of the design or a screenshot of a running program), links to
code, and any other resources that you think will help clearly convey your
design process.

This week, I've decided roughly what my syntax will be and chosen my host language. A lot of my thought process about design is above and in my design and implementation overview. I've had my backends chosen more or less since last week. Most of the implementation-related work I've done has been either reading or screwing around in the Python shell to see what works and what doesn't. I believe I now have a pretty good understanding of the parts of the TuxGuitar library I need to use, and have confirmed that I can call it from Python using pyjnius.

## Questions

**What is the most pressing issue for your project? What design decision do
you need to make, what implementation issue are you trying to solve, or how
are you evaluating your design and implementation?**
Since I'll be taking a fair amount of inspiration from VexTab and the backends are already fully-functional, I believe the most important thing for me to do this coming week is start writing the parser. In doing so, I'll need to start writing the intermediate representation as well.

**What questions do you have for your critique partners? How can they best help
you?**
The most important question I have for them is: what are your favorite and least favorite parts of VexTab's syntax? I need more than just my own opinion to decide where I should and shouldn't take inspiration from it.

**How much time did you spend on the project this week? If you're working in a
team, how did you share the labor?**

Counting studio time, I spent about 8 hours on the project.

