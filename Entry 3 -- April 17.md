# Design notebook for week ending April 17, 2016

## Last week's critique

I didn't receive a critique until yesterday, and most of the points it makes are relevant to before I externalized my dsl.

## Description

Ideas to maybe implement:

- multiple shapes per frame for primitives, or possibly a "complex shapes" section
- "inline" shape definitions in primitive definitions
- rule lifetimes: have a rule only exist from frame x to frame y (done for starting at frame x)
- more shapes (triangles? lines? arbitrary polygons?) (done for triangles)
- persistent vs non-persistent primitives
- being able to tell a rule to start rendering on a given frame. Possibly being able to do this in a looping manner, i.e., telling a rule to start rendering on frame 1, and then another copy on frame 2, etc. (note to self: this is going to probably be done by adding another argument (delayFrames) to chooseAndExecuteRule) (done except for the looping part)
- maybe a delayStart modification to allow the looping manner of the previous point

**TODO:** Fill in this part with information about your work this week:
important design decisions, changes to previous decisions, open questions,
exciting milestones, preliminary results, etc. Feel free to include images
(e.g., a sketch of the design or a screenshot of a running program), links to
code, and any other resources that you think will help clearly convey your
design process.

I'm spending some time just filling in loose ends that I need to - making it so that the program can automatically produce an mp4 with ffmpeg, allowing the user to specify the canvas size, etc. Apart from that I'm mostly just implementing features from the above list. I also did a bit of error checking and correcting (for allowing .5 instead of 0.5). I am a bit concerned about feature bloat, but some of these features seem very useful, and a lot of them are the result effectively of feedback from users who wondered if something was possible to do when it wasn't. 

I think the main thing my language needs to be as good as possible now is feedback on how good the syntax is, but that kind of feedback is more difficult to get. One person or a small number of people aren't really enough to get great feedback on syntax design because different users will use the language differently. I think I'm overall happy with the state it's in right now, though. 

## Questions

**What is the most pressing issue for your project? What design decision do
you need to make, what implementation issue are you trying to solve, or how
are you evaluating your design and implementation?**

Determining what my final product should look like. I've got it in a state where you can run it and get a finished video file, but it's through a python shell. I'd like to get it into a directly executable state but am not sure how, and I'm really not sure what else I want to get done before calling my project complete.

**What questions do you have for your critique partners? How can they best help
you?**

Really only one: what else should I be doing before calling my product complete? Are there any glaring 

**How much time did you spend on the project this week? If you're working in a
team, how did you share the labor?**

Not as much as the past two weeks. Maybe 4-5 hours total. I put in a lot of time in the past couple of weeks and my project seems to still be in a great spot so I wasn't feeling a lot of pressure to put in too much more time, and I wasn't entirely sure what direction to go in.
