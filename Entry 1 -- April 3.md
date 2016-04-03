# Design notebook for week ending April 3, 2016

## Last week's critique

The critique raises a good point that I didn't really have a detailed implementation plan, but I think this is partially just how I work. I didn't really want to make a detailed implementation plan because I think I work better by just doing things in the order that I want to get them working. I especially feel this way after this week because I think I am definitely ahead of schedule. I ostensibly have a working prototype, because what I've written so far is essentially an internal dsl (albeit a bad one because it's not supposed to be user-facing).

Some of the critique will be more relevant next week, particularly error messages - I haven't thought much about them, but they will be a challenge when I make the front end of the language. 

## Description

**TODO:** Fill in this part with information about your work this week:
important design decisions, changes to previous decisions, open questions,
exciting milestones, preliminary results, etc. Feel free to include images
(e.g., a sketch of the design or a screenshot of a running program), links to
code, and any other resources that you think will help clearly convey your
design process.

So I basically wrote the entire intermediate representation. Lots of exciting milestones, particularly that I can actually generate animations from the IR. Look in any of the animationtest folders to see some examples of things my program can create. 

In terms of design decisions, there weren't a ton of deep decisions; the majority of my work was really taking the ideas that had been ruminating in my head for the past weeks and expressing them formally in code. There were some key decisions, however. One was the way to lay out the class hierarchy with extensibility in mind. I'm hoping that with inheritance from shape, I'll be able to add more primitives if I want to later. Another big design decision/challenge was the idea of a RulesDict - it was one of the last things that I added in the class hierarchy. It was how I decided to translate the idea of ContextFree's recursive rule definitions into an intermediate representation: a dictionary of rules that can refer to each other by name when they need to invoke each other, and can keep track of modifications. 

Like I mentioned, my design process is essentially to just implement whatever thing I feel like implementing. I think my design plan wasn't really set in stone last week because this is the way I work best: I just go and do what I feel like doing because I am extremely productive when working on things that I want to be working on. I knew that I would be able to figure out the details as I went, more or less. I guess I took the advice of beginning to implement the design before planning out all of the details.

## Questions

**What is the most pressing issue for your project? What design decision do
you need to make, what implementation issue are you trying to solve, or how
are you evaluating your design and implementation?**

Syntax design. I need to figure out to what degree I want to just copy ContextFree. There are also other specifications (mostly for defining animation primitives) that I'll need to come up with methods for myself. I'll also need to do some research into possible parsing methods in python. 

**What questions do you have for your critique partners? How can they best help
you?**

The next real big step is to design the syntax, which raises a few questions. What are some of the biggest things from ContextFree that I should be copying? What things from it were undesirable that I should change? I also have a number of other questions:

* What's the best solution to the "infinite recursion" issue? ContextFree's solution is to allow both finite looping and recursive generation until shapes become too small to matter. I'm a bit concerned about this because one feature of ContextFree that I do not plan to copy is dynamic canvas resizing, so anything that's rendered off-canvas won't appear. I'm not totally sure how the lack of that will interact with this. The current measure in place is to just stop iterating after a fixed number of recursive calls.

* I have a lot more things that I need to specify as sort of metadata than ContextFree did: the start shape, the canvas size, the total number of frames, possibly the recursion depth limit, maybe the framerate. Is there anything to do with this other than just having something at the top of the file? At this point it seems like it would be nice to give users a templated file to work with that has all of this at the top.

* Are there any other important features in the IR that I'm missing? 

* Right now I have two specifications for translation: translate and rtranslate. Translate translates a shape by an exact amount; rtranslate translates by that amount, multiplied by the scale of the object, so that as objects get smaller they get translated less. Do I want to give users access to both of these? If so, how? I'm thinking I should pick one (probably rtranslate) as the "default" and give the other as a different option, something like "translate" for rtranslate and "translate_exact" for translate (not committing to these specific names by any means). 

**How much time did you spend on the project this week? If you're working in a
team, how did you share the labor?**

More than I'd like to admit. I've managed to view this project as not really being work, so I'm putting a lot of time into it which I don't mind. I'm really not sure how much exactly, I think definitely upwards of 10 hours.

