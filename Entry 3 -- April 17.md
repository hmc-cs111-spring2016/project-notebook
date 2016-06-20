# Design notebook for week ending April 17, 2016

## Last week's critique

Milo's critique helped me decide what feature to add next, and I think will help me decide what features I want to add next. He specifically suggested adding chords this week, since they're almost as common as single notes in guitar music (more common in some songs!). Aside from that, he suggested I just use a song I already know and try to write it out using my DSL. On following his advice, I realized I had completely forgotten to put in rests, so I addded those; I also chose to
add time signatures. Somewhat relatedly, Milo suggested that I consider letting users set a default note value for a song so they don't have to change durations for most note groups if most notes are something other than a quarter note. It was never my intent that a user would have to do that in the first place, and I think my old parser more or less worked as intended, but I realized a couple days after re-implementing my parser that the new one didn't carry durations over
from one phrase to another. I have since changed this, so if there are two phrases in a row of all eighth notes, but each is on a different string, the user only has to write "e:" once. This brings me to my new parser. I spoke to Prof. Ben in class this past week, and he said that most parsing libraries give the option to build something other than an AST in the process of parsing, but pyPEG (the library I was using) does not support this kind of functionality. By this point I
had written my intermediate representation in such a way that it is much easier (at least, easier for me to think about) to build it bottom-up--that is, start with notes and progress towards song-level--than it is to build it top-down. So, I've switched to pyparsing, which does give the option for building alternative structures. Milo was concerned that, if I output my intermediate representation directly from my parser, I would necessarily sacrifice flexibility. However, I've
actually found that once I've added a new feature to my IR adding it to the parser isn't very difficult, and once I've added it to the parser I don't have to do anything else. With pyPEG, I might've completely altered the structure of an AST and had to rewrite the code that walks it, but this is no longer an issue.

Milo also suggested that I include things like hammer-ons, and possibly also note/octave input (in addition to string/fret input). I definitely want to include both of those at some point, but I'm of the opinion that hammer-ons are less important to the "correctness" of a piece of tab than things like string/fret and duration. What I mean is, you can play something that's supposed to have a hammer-on without the hammer-on, and it'll still sound pretty similar, but if your
notes are all in the wrong places and have the wrong durations, it won't sound the same at all. Originally, I was going to leave out the note/octave notation completely since that wouldn't help with the original purpose of the language (i.e. checking tab). However, since one use-case for the language (when it's done, of course) will be faster creation of files for graphical tab editors, it does make sense to have note/octave after all. Like Milo said, a user might be trying to
convert standard notation to tab in addition to the other way around. I don't think I'll get to either of these features during the semester, but I hope to keep working on this project so I'll be sure to keep them in mind.

## Description

Probably the biggest change I made was a complete overhaul of my parser. I've described it above. Once I did that, I also added rests, chords, and time signatures (pretty sure I said all that above, too). To allow for multiple staves and let each have some staff-specific options, I moved a lot of the functionality that was in my IR Song class to a new Staff class.This week is sort of less obviously exciting than last week, but I believe that the overhaul of my parser will make it much
easier to add features to my language (it certainly has so far), which is exciting in its own way.

## Questions

**What is the most pressing issue for your project? What design decision do
you need to make, what implementation issue are you trying to solve, or how
are you evaluating your design and implementation?**
There are two features that I haven't implemented yet which I decided would be mandatory when I made my evaluation scheme. These are specifying the tuning of the guitar for a particular track/staff/whatever and getting some limited output to TuxGuitar. However, I've got a whole book of music from various Legend of Zelda games arranged for guitar, and there are a lot of tied notes and a decent number of triplets as well. I don't think adding tuning will take very long since my
intermediate representation basically already supports it, but the last time I tried to get TuxGuitar's utility library to cooperate I had a hard time since it's not very well documented. Should I keep adding language features at this point, or is now a good time to start working on TuxGuitar-compatible output? Should I even keep it mandatory in my evaluation scheme? (I forgot this wasn't the question section, but it's basically both the most pressing issue and what I
need help from my partners on.)


**What questions do you have for your critique partners? How can they best help
you?**
see above

**How much time did you spend on the project this week? If you're working in a
team, how did you share the labor?**

Wed: 5pm-6:45pm 7:40-9:20pm
Thurs: 3pm-4pm
Sat: 2:20pm-5:20pm
Sun: 2pm-3:40pm 4:40pm-6pm
This doesn't count the time I spent writing this evaluation, which was something like 6:10 to 7:25 but I wasn't really paying attention to what time it was when I started.
