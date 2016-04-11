# Design notebook for week ending April 10, 2016

## Last week's critique

Aaron warned me about my current error handling, saying that it went against what Prof Ben had told him.  He said that my "10-8" case (where I give a parse error since that's not a valid min-max integer range) is "semantic, not syntactic," meaning that I shouldn't return an error while parsing.  I don't understand exactly why this is, so it's something I plan to ask prof Ben/my critique group about before acting on it.

For note-lengths, Aaron suggested that I could enable users to specify probabilities for each length to occur (something I originally wanted to do) by using ties.  It's something we discussed in class, and would, as Aaron claims, solve the issue that I point out in last week's [notebook](https://github.com/milohan/project-notebook/blob/master/Entry%201%20--%20April%203.md).  However, depending on the note lengths the user chose and the time signature, this could cause there to be way more note ties than there would be in normal-looking music.  It would could also cause the generation of note lengths that the user didn't specify (something Aaron and I discussed in class).  For these reasons, I've decided to scrap the idea of allowing users to specify specific percentages for note-lengths, and instead focus on other parameters.  I don't think it'd be an especially useful parameter, and I'm starting to think it may not be possible to impement without annoying side-effects.

Aaron also responded to my question about the "Global Stave" from last week's notebook.  He said to scrap it, and I've done exactly that.  As he says, there isn't really any need for it since I can just put "global" parameters in the score environment of the program.

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

####  This week's work log (feel free to ignore):

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
- Also - There are a huge amount of things I could work on to try to generate harmonically pleasing music, but I'm going to leave that to the side for the time being.  The code I'd have to write would be super back-end heavy, and would probably be out of scope considering the amount of time I have for this project.  I would like to do some basic things with harmony/consonance before the semester ends, but at least for the next week I'm going to focus on things like user feedback, the UI version of the DSL, rhythms, MIDI output, etc.

**What questions do you have for your critique partners? How can they best help
you?**

- What are your thoughts on my idea for generating rhythms?  Any major flaws I haven't noticed?  Any other ideas on how I could get accetable rhythms?
- Can you think of a better parameter name than "polyphony"?  It describes the number of notes that can be generated in a single chord.
- For parameter names that are more than one word, such as "absRange," what do you think of using camel case?  I write a lot of java, so it feels natural for me, but would something else be better?
- Are there any major parameters I should prioritize (other than rhythm)?  There are countless I can think of, but there are also a bunch of other things I'd like to work on.
- Besides parameters, what should I prioritize?  I feel like there are a lot of directions I could go.
	- UI version of DSL
	- Better user feedback (e.g. highlighting problematic code)
	- MIDI integration (allow users to hear what the music sounds like)
	- Something else?

**How much time did you spend on the project this week? If you're working in a
team, how did you share the labor?**

Was able to spend some time coding/planning every day this week (github commits won't necessarily reflect this since I'll often work past midnight and I sometimes forget to push).  Splitting up my time into smaller chunks helped keep things moving along.  If I got stuck on something, I'd just sleep on it and come back the next day.

Total: ~14 hours.  Daily breakdown in work log above.