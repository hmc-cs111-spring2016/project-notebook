# Design notebook for week ending April 24, 2016

## Last week's critique

Last week in my notebook entry I spent a long time discussing a frusterating error I was getting in my code, but I actually solved the problem in class on Monday and talked to my critique partner so that portion of their critique was no longer relevant. They had several good ideas for me, which I am still hoping to look into in the last few days I have to work on this, including reading in the Scheduler App input from a file so that the user can generate several possible sets of classes that they want to consider seperately and just run each seperately, I could also extend functionality to allow the user to give additional preferences from the command line and then regerenate their schedules in case they want to just make slight modifications to their preferences and see how it changes the ordering of possible schedules. I spent a while looking into alternative representations of the course schedules because of how frustrating all of the embedded lists were when I was trying to debug. However, I solved all the problems with the lists and decided that the time and effort which would be required to change the entire underlying representation of the data in my language would not be worth it at this late point in time. 

I wish I had perhaps started with a different initial data structure, because this may make it difficult for anyone except me, since I have spent so much time working with it, to really track the way the information is passed through all the methods and therefore makes the code less extensible, and more error prone for people who are trying to extend it. I have spent a lot of time this week considering extensibility issues and I think that the most confusing parts of my code, which would make it the hardest for someone else to work with it and extend it, are keeping track of the number of embedded lists and keeping track of the time comparison functions which is where I found errors last week, because there are so many layers of them.

## Description

I decided to allow people to rate time blocks with a hard start and end times, so if you rate the time block from 10 AM until noon and you have a class that goes from 11-12:15 it will not recieve that rating. Time ratings are also independent of day ratings, which allow you to expesss a general dislike for classes during a specific time, but does not allow you to specify your preferences for a specific time during one day. I also implemented error checking for the user, so that they would recieve error messages defined by me if they put invalid input in their code. I found several logic errors and corrected them, and changed the way the user runs their program, to remove any extraneous Prolog code from the user's file so that they only need to use methods which I created for them. This was actually the most difficult part because I didn't really like the added complication when I tried to run the code.

## Questions

**What is the most pressing issue for your project? What design decision do
you need to make, what implementation issue are you trying to solve, or how
are you evaluating your design and implementation?**

**What questions do you have for your critique partners? How can they best help
you?**

I am still very unsure about what makes the most sense for the ways in which people should be allowed to rate times, because there are so many possible things that people might mean to say and so it is very difficult to determine which way of understanding a time rating would make the most sense to implement.

As I wrote on piazza,

I want to allow people to rate classes based on times. Perhaps people hate morning classes, or have work at a certain time and don't want to have a class during that block etc.
 
Here are the options for how to do this which I have thought of:
 
1) rateTimeBlock(StartTime, EndTime, Value)
The first would be to just rate a timeBlock like this. This seems to be the most intuitive way, but could cause problems, for example, if someone wrote rateTimeBlock(9,10,-20) then classes that went from 9-9:50 would obviously have a value of -20, but what about classes that go from 8:10-9:25 or 9:35-10:50 or a super hum that is 3 hours long that goes through that time period, should those also receive a rating of -20?
 
2) rateMorning(Value), rateAfternoon(Value, rateEvening(Value)
I could have predefined time periods which they could give a value to, but this would be less flexible for my users and might not allow them to say everything they want to.
 
3) rateStartTime(smallerTime, greaterTime, Value), rateEndTime(smallerTime, greaterTime, Value)
If someone didn't want any classes before 10 AM they could put in rateStartTime(8,10,-20) which would check if any class started between 8 AM and 10 AM and if so, give it a rating of -20. However, if you did not want to take any class between 3 PM and 4 PM (15 and 16 military time) that would be much harder to say.

Do you have an opinion? Or a different idea?

**How much time did you spend on the project this week? If you're working in a
team, how did you share the labor?**

I spent about 
	- 5 hours on Friday
	- 3 hours on Saturday
	- 5 hours on Sunday


