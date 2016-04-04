# Design notebook for week ending April 3, 2016

## Last week's critique

This week I spent most of my time working on refamiliarizing myself with Prolog and setting up the tools I would be using. I read through some of the information on the two pages that my critique partner provided links too, but they weren't directly relevant to the work I did this week. They should be helpful this week as I begin to implement the actual schedule generating algorithm. My partner expressed some concerns about the ease of use of Prolog for my users, but Prolog is in general an extremely easy language to write simple rules in. It is very intuitive because you simply make statements about facts and all of the more difficult programming would be done by me to run in the background and generate the course schedule from the simple input the user gave. Also, since I am writing a DSL, my users will be using my language, not general Prolog, although since Prolog is already close to a DSL it may not be to far off from normal Prolog syntax.

My partner suggested that I contacted Casey Chu, the student who designed the Scheduler App. I actually did that when I first began to consider this project and met with him this week to discuss his work and my ideas to extend it. WIth Prof Ben's approval, he will be helping me link my language to his existing program by providing perhaps a simple button at the bottom of the scheduler app page which will generate class data in the correct format to be read into my program so I will not need to deal with portal or make my users generate that data by hand to input to the program.

## Description

The code I have written so far is in inputReading.pl in the CourseScheduler project. Instructions to run it are provided at the top and a sample input list of courses can be copied and pasted from the bottom of the file into the terminal window if you need an example schedule.

I spent quite a while this week trying to get eclipse to work with prolog to make testing and debugging easier on myself. I made myself give up on that idea Thursday night when I realized the time it was consuming was far greater than the benefit I expected to get from it. I instead installed a plugin for sublime that provides a nice interface for working in Prolog. 

I then spent several hours refamiliarizing myself with Prolog and learning how to write basic rules in it, before I began to do any actual code.

The biggest thing I discovered this week was how to read in the class data and turn the information into rules in my database with corresponding values that could be modified by the user to show thier preference. I talked with Casey Chu, the designer of the HMC Scheduler app this week, and with permission from Prof Ben, Casey has agreed to add a link at the bottom of the scheduler page that will generate a list of the classes you have chosen on the scheduler app in the correct format to be put into my Prolog program, so I will not have to deal with Portal at all, and hopefully my work can be extended to work well with his app.

## Questions

**What is the most pressing issue for your project? What design decision do
you need to make, what implementation issue are you trying to solve, or how
are you evaluating your design and implementation?**

The next big issue for me to tackle is the comparison logic for the schedule itself. I need to compare the day of the week for each class. If they occur on the same day, then I will have to see if the time the class is at conflicts with another class and use that when I generate my schedule. I will also need to write the actual function that will find the schedules with the highest ratings and generate them for the students. I have already written most of the function that provides the value a given class adds to the schedule but not the function that will choose which combination of nonConflicting classes will generate the highest value schedule.

**What questions do you have for your critique partners? How can they best help
you?**

So far, I have not implemented very much of the comparison logic. I have set up the data base, and made the rules to allow people to specify their preferences for a wide range of elements pertaining to a class including based on the professor, the time the course is offered, day it is offered on, section, or time if falls during. Can you think of other things someone might wish to rate their possible classes based on? 

Also, I am currently implementing this in generic Prolog and then translate it into a DSL that simplifies how students will input their preferences, but do you have any feedback for me on my first idea for function names? Can you guess what these are? What they do? Would you remeber this syntax easily. So far, the functions I have written that the students would directly use are:
	-rateProf(Name, Value)
	-rateClass(Name, Value)
	-rateSection(Class, Section, Value)
	-rateTimeBlock(Start,End, Value)
	-rateDay(Day, Val)
	-setDayMaxHours(Day, Val)
	-setAllDaysMaxHours(Val)

This is a rather vague, but I am also a little nervous, so far there are not a lot of different ideas my users can express and it doesn't have a very strong language feel, some of that is simply that i haven't finished implementing a full program even in generic Prolog yet, but do you have any insight into things I could add that might give it a stronger language feel?

**How much time did you spend on the project this week? If you're working in a
team, how did you share the labor?**

I spent about 15 hours working this week:
	- 2 hours on Wednesday.
	- 3 hours on Thursday.
	- 3 hours on Friday.
	- 4 hours on Saturday 
	- 3 hours on Sunday
	
