# Design notebook for week ending April 3, 2016

## Last week's critique

I got some great feedback/suggestions from William and Prof Ben about user experience and adding more language-y features.  
William's main concern was that my project seems too backend-heavy.  I definitely was a bit worried about this since I started the project, so this served as a confirmation that I'll have to come up with some more language-y features.  However, he warned against adding things like complex control-structures simply for the sake of having more language to design.  This is something I'll be keeping in mind - user experience comes first.  
He also suggested that I eventually implement some sort of "beginner" setting where users don't have to write too much code.  I kind of liked the sound of this, but wasn't sure exactly what this would look like.  
However, Prof Ben then suggested that I eventually implement an alternate UI/GUI-based form of input.  This seems like a great idea that would address both of William's suggestions.  It's a language-y feature that should be a lot simpler to use for beginners.  I'm not yet sure exactly how it will look/work (I'm thinking it'll involve a bunch of sliders and dropdowns), but it's something I'll be thinking about in the coming weeks.

## Description

I was able to make a decent amount of progress this week.  Most excitingly, I've linked up all the various technologies/tools and laid out the architecture of the DSL from input to output.  The biggest factor that allowed me to get this all done was the simplicity of PEG.js.  I thought the parser was going to be a roadblock (it still might, later down the line), but so far it hasn't been.  Check out my parser grammar [here](https://github.com/milohan/sheet-music-gen/blob/master/parserGrammar.js) if you're interested.  
You can test out what I have so far in terms of the actual DSL by heading over to my [code repo](https://github.com/milohan/sheet-music-gen).  Just open up the index.html file with your browser of choice (more detailed instructions can be found in the [readme](https://github.com/milohan/sheet-music-gen/blob/master/README.md).  
Below is a work log outlining the tasks I worked on throughout the week.

Tues
- familiarized myself with PEG.js, abcjs
- implemented logic to generate and render arbitrary number of measures of music
	- currently using random pitches, only quarter notes
- started building parser with PEG.js
	- now accepts "measures: 12"
- stitched together basic architecture, from input to output
	- user input -> parser -> internal logic -> render sheet music

Wed
- (during class) worked on design + implementation writeup

Fri
- parsing + logic for measure ranges ("measures: 12-14")
- parsing + logic for keys ("key: A")
- parser now accepts arbitrary number of lines of parameters

Sunday
- finished design + implementation writeup
- notebook entry

## Questions

**What is the most pressing issue for your project? What design decision do
you need to make, what implementation issue are you trying to solve, or how
are you evaluating your design and implementation?**

The next set of parameters I'm hoping to tackle are the ones relating to rhythm.  I'd like to, for example, allow users to specify that they want 30% quarter notes, 20% half notes, and 50% eight notes.  However, I'm not exactly sure how I'll accomplish this in the back end.  If I simply randomly choose note lengths based on the given percentages, the results will be skewed because each measure has to have an exact number of beats.  For example, let's say the user asks for 50% half notes and 50% quarter notes in 3/4 time.  With the above method, we'd have a 50% chance of choosing a half note as the first note of each measure.  However, any time we choose a half note first, we'd then have to automatically stick in a quarter note to complete the measure (since you can't fit two half notes in a measure when you're in 3/4 time).  We'd end up with more than 50% quarter notes.  There's probably a better way to approach this problem, but I'm having some trouble coming up with it.  If there isn't, I may have to scrap the feature of allowing users to enter specific percentages.

My original plan was to have three places for users to enter parameters:
1. Inside a "Stave" (bounded by brackets).  Parameters here would describe a specific stave.
2. Inside a "Global Stave" (bounded by brackets).  Parameters here would be inherited by all staves (though individual staves can override these global parameters by defining them themselves).
3. Outside any staves (not bounded by any brackets).  These parameters are "score parameters." These are parameters that aren't specific to any stave, such as the number of measures or whether or not there should be repeats.

However, now I'm wondering whether there's a need for the "Global Stave" at all.  I could instead have users enter "global" stave parameters outside any specific stave, along with the score parameters.  This could be clearer/simpler, especially for non-programmers who might have trouble understanding what a "Global Stave" is supposed to do.

**What questions do you have for your critique partners? How can they best help
you?**

My questions are basically if you have any advice for the problems I outlined above. Are there any algorithms or ideas that relate to my note-length issue?  Do you think it's even necessary that I allow users to enter specific percentages?  Are there any other pros/cons you see to having a "Global Stave"? Should I have it or not?

**How much time did you spend on the project this week? If you're working in a
team, how did you share the labor?**

Including time spent on writeups + design, I spent about 11 hours on the project this week.