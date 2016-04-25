# Design notebook for week ending April 24, 2016

## Last week's critique

Milo's critique was extremely helpful again this week. The feedback he gave me more or less laid out what exactly I should focus on this week (which I for the most part have completed). 

His feedback suggested a few language features that would help differentiate my DSL from existing GUI applications; more specifically, he suggested that I create some kind of history command that allowed users to see a "browser history" of sorts for edits they have made to the song. Given that my DSL is entirely text-based, I'm able to convey the history of edits in a rather succinct and clean way. 

Building off on this history feature, he also suggested that I implement a revert feature that would allow users to go back to a specified point in the history. He also was kind enough to give me a few drawbacks that I have also addressed. These include dealing with history after reverting. After some consideration, I decided to simply show the edits of the reverted song. I felt like it would be redundent and overly-complicated to show the "revert" step as a part of the history, and allow users to "revert" their "revert", "revert" the "revert" to revert the earlier "revert, and so on...

A non-feature specific suggestion Milo also gave is to more thoroughly consider boundary conditions and display errors rather than simply having the REPL interface crash. I have thoroughly addressed most errors that the average errors will run into and have written accompanying error messages. 

The last suggestion that I've addressed from Milo's critique is allowing play() to run on a separate thread. I've completed this as well, as I felt this was a particularly important feature from the user experience perspective. I'll go into more detail on this in the next section.

Because of Milo's specific and effective critique, I changed a few feature designs of my REPL interface and have since implemented them. I decided to put off concatenation and other more back-end features as Milo's suggestion felt more focused on language design. I'll explain in more detail in the next section, but working this week I realized a lot of the original goals I had for this language were largely centered around the back end and less on the language design, which is something that I've adjusted for as I have been developing my DSL. As such, the product of this project is not what I expected, but, to me, better aligned with the goals and purpose of this class.

## Description

I've attached a screenshot of my shell with the working "history" and "revert" feature as explained in the above section.
![alt text](https://github.com/williumchen/project-notebook/blob/master/Screen%20Shot%202016-04-24%20at%2010.49.54%20PM.png)

This week my task was split amongst a variety of things based off of Milo's suggestions:

*  Cleaning up the error handling and error messages. I missed a lot of edge cases last week as I had just implemented the undo feature among other things. A few errors that Milo mentioned including negative indexing with the undo feature (undoing when there were no more undos). I have addressed all the errors Milo mentioned and have also (hopefully in good faith) implemented error messages and handling for all the new features this week. These include undo and revert being consistent in that you can revert to a certain step and undo afterwards, and the song will update correctly. This was more challenging than I had originally expected as history, undo, and revert all had to be consistent with each other. More generally, I also realized I had to add several try exception catches to ensure that the REPL interface never crashed out and simply printed an error message for the user. A few methods that I've changed include the play feature, the load feature, and the save feature.
* Implementing the history feature. As shown above, typing history gives a nice and easy to read list of edits made to the current song being editted. This involved creating a new list that kept track of command history, as I could not use the song history list which kept track of the actual audio file with the editted changes. 
* Implementing the revert feature. This was a obvious follow-up to the histor feature. Again, as mentioned previously, making this consistent with the history and undo feature proved more troublesome than actually implementing. As I had already implemented undo, it was more or less just calling multiple undos.
* Added a separetely threaded play feature. Originally, the REPL interface would hang while a song was playing. Now that play is being run on a separate thread, the user is able to continue making edits, checking history, or whatever they want while the song plays in the background. I felt this was particularly important as having the REPL interface hang for 5 minutes or however long the song in question is would be very bothersome and unproductive.
* Fleshed out docstrings for all methods and just made the code overall more readable. Added better file organization with folders. 
* Added *,/,+,- to volume as I felt this was easier to understand. Before, I simply had "+" which would increase the volume by a factor. I felt factors were easier to understand, but Aaron and Milo suggested I added something similar to how volume adjustment works for computers, which is by fixed decibels. As such, I added * and / to handle changing the volume by a factor (e.g. doubling the volume) and + and - to handle changing the volume by a fixed amount (increasing and decrasing by 5 dB)

## Questions

**What is the most pressing issue for your project? What design decision do
you need to make, what implementation issue are you trying to solve, or how
are you evaluating your design and implementation?**

With every feature I implement, I feel like there are more things to do every week (which is reflected in the advice I gave for future-DSL students). There doesn't seem to be any particularly pressing issue for the project as much as there are simply too many things that can still be done, or things I'd like my language to do, but don't have the time to do. A few obvious ones are fleshing out the back-end with more editting features. A big one that I originally listed as a stretch goal but don't think I'll have time to address is allowing users to define their own effects by mixing together existing effects in the language. I really liked this idea but it had several preliminary steps that needed to be completed. With enough time, this feature, I feel, would make my DSL pretty extensible. For now, my focus is mostly on user experience, not so much the back-end tools. While the user can't necessarily do much to the actual audio file, what the user can do, he/she can do rather easily!

**What questions do you have for your critique partners? How can they best help
you?**

Do you have any other suggestions for the REPL interface? (I ask this question every week, and the critiques I've been getting has been really helpful)

How do you feel about the syntax design for features that I've implemented this week?

**How much time did you spend on the project this week? If you're working in a
team, how did you share the labor?**

I spent roughly 11 hours, including in-class time. This includes writing up my critique, writing up this notebook entry, and implementing all the features I discussed above.
