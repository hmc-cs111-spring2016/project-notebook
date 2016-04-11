# Design notebook for week ending April 10, 2016

## Last week's critique

**TODO:** Fill in this part with a summary and reflection on the critque you
received for last week's work. Answer questions such as:  How, specifically, did
the feedback help improve the project? Did the feedback point out or offer
something you hadn't considered? Did it help you make a design decision? Was it 
helpful in addressing the most pressing issues in your project? How will you
incorporate the feedback into your work? Will you change something about the 
design, implementation, or evaluation as a result?



## Description

The major things I worked on this week are as follows:
- Pitch: Started creating an internal system for representing pitches that allows me to perform meaningful logic on notes.  I decided to base my system on [MIDI pitch values](http://www.electronics.dit.ie/staff/tscarff/Music_technology/midi/midi_note_numbers_for_octaves.htm) (integers from 0 to 127 representing pitches).  By using integers, I can perform simple harmonic "math" that is key-agnostic.  For example, to create a 5th interval I add 7 to a pitch's MIDI value.  This system allowed me to implement the "absRange" parameter that defines the range of allowable pitches.  In order to use MIDI in the back-end, I convert [scientific pitch notation](https://en.wikipedia.org/wiki/Scientific_pitch_notation) (input) to MIDI value (internal) to ABC notation (output).
- Polyphony (the number of notes that can be played at the same time): Back-end was straightforward, though I'm sure it will get more complex once I allow for parameters like "relRange" (number of allowable semitones between notes within a chord).  [Parsing](https://github.com/milohan/sheet-music-gen/blob/master/parserGrammar.js) for this was interesting.  I allow users to specify polyphony in three different ways.
	- A single integer: "polyphony: 3" means that only triads will be generated.
	- A range of integers: "polyphony: 1-3" means that single notes, diads, or triads will be generated.
	- A list of integers: "polyphony: "1, 3" means that only single notes and triads will be generated.
- Multiple staves/voicings: Users can now create an arbitrary number of staves.  Followed my critique group's advice and scrapped the "Global stave" idea.  Now, parameters defined outside any specific stave are automatically considered "global" and will be used by any stave that doesn't itself specify that parameter.

#### Example input and corresponding output:

![asd](https://raw.githubusercontent.com/milohan/project-notebook/master/images/4-10_input.png)
![output](https://raw.githubusercontent.com/milohan/project-notebook/master/images/4-10_output.png)

####  This week's work log:

**Monday**: 3 hours  
-wrote critique for William  
-users can now specify seed with "seed" parameter.  
-"clef" parameter now supported.

**Tuesday**: 1.5 hours  
-designed and began implementing pitch system, from input -> internal -> output

**Wednesday**: 2 hours  
-parsing for "absRange"

**Thursday**: 2.5 hours  
-worked on back end for pitch system  
-"absRange" backend working  
-started working on polyphony support (chords)

**Friday**: 1.5 hours  
-parsing, semantics for polyphony.  Allows int (eg "4"), range of ints (eg "1-3"), list of ints (eg "1, 2, 4")

**Saturday**: 1 hr  
-planning + parsing for multiple staves

**Sunday**: 2.5 hr  
-backend + finished parsing to handle arbitrary number of staves  
-"global" variables (defined outside of staves) properly used when not defined inside individual staves

## Questions

**What is the most pressing issue for your project? What design decision do
you need to make, what implementation issue are you trying to solve, or how
are you evaluating your design and implementation?**

- I'm still working out how I'll generate decent rhythms while allowing users to specify any time signature any any set of note lengths.  An idea I had was as follows: on a measure-by-measure basis, start out with largest available note (e.g. a whole note in 4/4 time, assuming the user allows it).  Based on some probability, either leave it or split it into equal-length children.  Recursively apply same algorithm to children.  A benefit of this is that it should naturally create rhythms that emphasize downbeats.  However, it needs a lot of refinement if I'm to use a version of it since certain problems would arise.  
	- Some note values would be impossible to generate.  How do we get, say dotted half notes in 4/4 time?  (Maybe allow a certain chance for two notes to "combine" into one once the measure's been generated?) 
	- How does this alg work in odd times?
- For user feedback, I need something better than the types of error messages I'm currently giving.  I'm contemplating trying to implement feedback where I'd highlight the troublesome pieces of code.  I'd have to delve into the parser generator I'm using, but it could be a cool "language-y" aspect to sink some time into.

**What questions do you have for your critique partners? How can they best help
you?**

- What are your thoughts on my idea for generating rhythms?  Any major flaws I haven't noticed?  Any other ideas on how I could get accetable rhythms?
- Can you think of a better parameter name than "polyphony"?  It describes the number of notes that can be generated in a single chord.
- For parameter names that are more than one word, such as "absRange," what do you think of using camel case?  I write a lot of java, so it feels natural for me, but would something else be better?
- Are there any other major parameters I should prioritize?  There are countless I can think of, but at some point I want to start working on the UI version of the DSL, since that's more design/language-y and should be refreshing to work on.

**How much time did you spend on the project this week? If you're working in a
team, how did you share the labor?**

Was able to spend some time coding/planning every day this week (github commits won't necessarily reflect this since I'll often work past midnight and I sometimes forget to push).  Splitting up my time into smaller chunks helped keep things moving along.  If I got stuck on something, I'd just sleep on it and come back the next day.

Total: ~14 hours (daily breakdown in work log above)