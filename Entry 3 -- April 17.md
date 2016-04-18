# Design notebook for week ending April 17, 2016

## Last week's critique

Aaron's critique was helpful in giving me a sense of alternatives to the ideas I proposed last week. However, after careful consideration, I have decided against following his feedback. This is not to say his feedback wasn't particularly helpful, but I felt (as he pointed out as well) his suggestion would have been significantly more difficult to implement and deal with. More specifically, as he mentioned as well, the lack of familiarity with the project really limited how specific his suggestions could have been, which I completely understand. Aaron gave me several suggestions, most of which I had not originally considered. In particular, he suggested that rather than storing intermediate states of the editted file, it would be a better idea to store the modifications and undo them. Because the back-end relies heavily on pydub, and most of the audio manipulation does not directly involve me changing amplitudes / frequencies, undoing manipulations would be much more difficult than simply keeping intermediate versions. Aaron's critique did allow me to realize it wasn't the best idea to store several intermediate versions, so in that sense his feedback did help me come to a design decision of sorts. Although not directly addressed, I have implemented a feature in my REPL interface that allows users to record their commands in a .cmd and allows them to playback the commands by loading said .cmd. 

With regards to the intermediate representation, I have decided to go with a Command class that has an action, effect, and value attribute. This will most likely be extended further for the following week, but for now, it serves its purpose as I can easily integrate the IR with the parser and the REPL interface. 

## Description

**TODO:** Fill in this part with information about your work this week:
important design decisions, changes to previous decisions, open questions,
exciting milestones, preliminary results, etc. Feel free to include images
(e.g., a sketch of the design or a screenshot of a running program), links to
code, and any other resources that you think will help clearly convey your
design process.

I spent a majority of this week getting the REPL interface to work as well as fleshing out and hooking together each component of my DSL to use the REPL interface. Here's a screengrab summarizing how the REPL interface works and a few implemented features of my DSL. 

![alt text](https://github.com/williumchen/project-notebook/blob/master/Screen%20Shot%202016-04-17%20at%2010.34.27%20PM.png)

As of now, the parser, intermediate representation, and back-end are "complete," but in a very bare bones sense. Currently, the two tools I've gotten to work with the REPL interface are volume adjustment and pitch adjustment. However, this doesn't strike me as a huge concern, as I think I've done a decent job of focusing on the more language-design aspects (I plan on talking to Prof. Ben about the direction I'm going). I feel like adding more tools will ultimately be a more back-end heavy thing. I've also implemented a few changes to volume and pitch adjustment to make it more user-friendly. Before, the volume adjustment was in decibels (dB), which is a rather unintuitive unit. I have since converted to a factor conversion (e.g. making something twice as loud vs 5 dB increase), based off of the approximate ratio of 6 dBs ~ doubling the volume. For this week, I plan on adding additional tools that will require me to further develop my parser, intermediate representation, and backend. As of now, the parser takes in 'rules' of the strict form 'action' -> 'effect' -> 'value' (e.g. + volume 3), which isn't the most creative or interesting. I'll be looking into more pyparsing examples this week for some inspiration. It feels like the parsing scheme I have now is pretty rigid and I think I'll definitely need to work on it more this week.

In terms of the REPL interface, after some research, I decided to use the [cmd](https://docs.python.org/2/library/cmd.html#cmd.Cmd) library, which made creating a shell for my DSL a bit easier. A pretty significant problem I ran into was that pyparsing doesn't seem to necessarily be made for command line parsing. In fact, there are command-line specific parsers. I ran into an awkward problem where using a command-line parser like shlex or argparse might have made the interaction with cmd a bit nicer, but at the same time I needed the flexibility pyparsing provided to ensure that the DSL looked natural and not like your typical command line commands (e.g. python parser.py -volume -increase 30). I ultimately managed to get everything hooked up (the parser using pyparsing, my intermediate representaiton, and the backend using pydub), such that it works as intended with the REPL interface. 

A few features I added specific to the REPL interface include a nifty help documentation that can be called with 'help' or '?' when running my DSL shell. These features are also highlighted in the included screenshot above. Additionally, I have implemented 'load', 'save' and 'undo' functionalities, which are specific to my DSL. They are pretty self-explanatory: 'load' loads a sound file to be editted, 'save' saves the changes made as a file-name of your choosing, and 'undo' undoes the most recent edit. They aren't the most robust yet, and there are obvious improvements. For instance, one thing I plan on working on is to provide users the ability for more specific 'undo's. Another thing that can be improved is loading multiple sound files. This ranks pretty high in the priorities of things to be worked on, as I think it'll be an interesting language design challenge as well as genuinely useful feature for users. Also, one thing that I can't seem to get to work is the 'play' feature. As of now, the shell hangs while the music is playing, which is how 'play' is implemented in pyaudio. I don't think there's a way around this, but if anyone knows, some feedback would be great here. I tried using a try/except for KeyboardInterruption, but this doesn't seem to work. 

## Questions

**What is the most pressing issue for your project? What design decision do
you need to make, what implementation issue are you trying to solve, or how
are you evaluating your design and implementation?**

As aforementioned, the most pressing issue for my project right now appears to be creating a more robust parser and implementing additional tools beyond volume and pitch control. I'm glad I finished working out the REPL interface for now, as it was quite time consuming. However, I feel like it's a pretty big part of the language-design aspect, so it felt more in-line with the class objectives. As the following section will make clear, I'd like to have a few questions answered regarding the parser, the REPL interface, and additional tools.

**What questions do you have for your critique partners? How can they best help
you?**

* I'm curious how you feel about the REPL interface. I want to hear any suggestions / critiques. I think I've implemented most of the tools that Milo had suggested to me last week, but there's always room for improvement. As a potential user, (independent of the tools), what features would you like the interface to also provide?
* How do you feel about the current parsing model? I think it's really strict and makes it difficult to parse anything else other than text in that form. Do you have any potential ideas? I know it's hard to generate ideas without understanding pyparsing. I've been having a difficult time with pyparsing, so it's definitely something I'll need to look into. 
* What tools can you think of that might consider to be "essential"? What syntax might you want to see with these tools? I have a few in mind, (reverb, delay, concatenating 2 different music files, chopping up a music file) but I'd like to hear from a user perspective.

**How much time did you spend on the project this week? If you're working in a
team, how did you share the labor?**

I spent approximately 13 hours on the project this week. I spent 2 hours earlier in the week working on the critique for Milo. I spent 2 hours on Wednesday working on the REPL interface, and looking into [cmd](https://docs.python.org/2/library/cmd.html#cmd.Cmd) as a way to create my DSL as a line-oriented command interpreter. I spent 8 hours the remainder of the week on Friday, Saturday, and Sunday working on getting the REPL interface working with my DSL. I have since implemented the volume and pitch features with the REPL interface as well as load, save, undo, and exit. I have also created documentation for my language in the shell that can be called by typing "help". I spent approximately an hour on writing this notebook entry. 
