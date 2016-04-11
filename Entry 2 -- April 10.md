# Design notebook for week ending April 10, 2016

## Last week's critique

**TODO:** Fill in this part with a summary and reflection on the critque you
received for last week's work. Answer questions such as:  How, specifically, did
the feedback help improve the project? Did the feedback point out or offer
something you hadn't considered? Did it help you make a design decision? Was it 
helpful in addressing the most pressing issues in your project? How will you
incorporate the feedback into your work? Will you change something about the 
design, implementation, or evaluation as a result?

The Critique gave me lots to think about this week, although most of it will probably be more applicable in this coming week. My Critiquer suggested I spend some more time thinking about who my audience is and how they will feel interacting with my language which will be important as I begin to try to hide some of the Prolog "flavor" in my DSL. I would love to work to get the class info and read it into the program behind the scenes so that the user doesn't have to deal with it at all, but I would need to learn a whole other programming language (or 2!) based on my discussion with Casey, so I don't think that will be within the scope of this project. I already have lots and lots to learn just because i am trying to break all of my imparative programming habits to use Prolog!

I am not totally comfortable using Eclipse, but I was trying to use the ProDT plugin which seemed very helpful, but was just not allowing me to run the programs easily. Professor Ben spent some time with me, trying to get it set up, but the pathway we set up was messed up when I tried to move one of the files and I decided it wasn't worth spending more time on getting Eclipse to work correctly, when I could just install a simple extension for Sublime.

Last week, I made the decision to format all of the class information in lower case, which I felt good about, but the syntax of the class information that Casey generated for me is normal upper case in single quotes, so I will just modify my code to use that because it seems to be about equivalently intuitive and that way it will be consistent for my user. It wasn't my first choice, but I don't think that it is a bad choice and as you suggested I could probably write a small external DSL over this at some later point if I wanted to simplify it even more. 

The military time format worked very well, especially since the syntax Casey gave me already converted the days to numbers, seconds to fractions and hours to military time.

It might be useful to be able to rank a department, but that would be a later addition since currently the only departmental information i am taking in is simply part of a section number.

## Description

This week I began developing the algorithm that runs once the user has finished specifying all their preferences to generate the optimal schedules options I learned so much about debugging Prolog code, like that if it can't find a function you clearly defined in your code it may because you are missing a ',' at the end of a line in its rule, or that you left off a parenthesis so its not finding the right number of arguments. I discovered the clpfd library which is extremely helpful in doing comparisions and such, and several built in functions like findall, which helped me generate all possible schedules. I borrowed heavily from the ideas and thought process articulated at these two websites [Here](https://newtocode.wordpress.com/2013/11/23/knapsack-problem-in-prolog/)  and [Here](https://rosettacode.org/wiki/Knapsack_problem/Bounded#Library_clpfd) which explain how to solve a basic knapsack problem in Prolog. Looking over their code gave me ideas about how to express these types of things in a declarative rather than imparative programming language. My algorithm seems to be working correctly, but I have not yet determined how to remove duplicate schedules that my program is generating.

## Questions

**What is the most pressing issue for your project? What design decision do
you need to make, what implementation issue are you trying to solve, or how
are you evaluating your design and implementation?**

There are currently still a lot of holes in my projects implementation, in part because learning to debug Prolog, without being able to go to other students or grutors for help (since most don't remember Prolog) is very difficult and takes a long time. This week, I would like to determine how my user can write their program in a seperate file and include my code so it will run correctly. I need to add a condition to my time conflict clauses so that 1st and 2nd half semester courses wont appear to conflict. I need to add a credit maximum that the user can manipulate rather than having it hard coded in, and determine how to deal with the way different colleges determine the number of credits a class is worth, and work on the format of my DSLs output to a user, and begin considering how to remove the Prolog "flavor" from my language. Once Casey gets me the final version of the Scheduler output syntax, I will need to finish modifying my input functions so that the correctly read in and store all the data.

**What questions do you have for your critique partners? How can they best help
you?**

I am kind of stuck as to why my program is outputting multiple copies of the same schedule, although I think it might be because of the subsequent function the bestScheduleAlg.pl file. It generates all the possible sublists of classes, but I am not yet sure how to make  it only generates unique sublists. If you have ideas for me on that, I would appreciate them! 

Beyond that, if you have any ideas for better names for my 5 functions that together make sure a set of classes comprise a valid schedule -- singleClassConflict, singleTimeSlot, classConflict, noTimeConflicts and singleTimeConflict -- (all in bestScheduleAlg.pl), I would appreciate it. Debugging was difficult, because I couldn't figure out which function was supposed to do what, and the harder the code is to read, the harder it will be to come back later, modify it, and/or extend it.

I am also hunting for useful tools for outputting the course information in a readable format and am hoping for any ideas you have about which part of the course information should be output. Does it make the most sense to just output course name and section? The professors name or the time? the total value you gave each class in your schedule? Which information would you want to see?

Lastly, I am not sure how to allow my users to include my code in whatever file they are writing their programs in.This is pretty important if I want my language to really be useful and I am hoping to do more research this week on that but if you find any good resources for me, I would appreciate them!



**How much time did you spend on the project this week? If you're working in a
team, how did you share the labor?**

I spent approximately 14 hours working on the project this week

Friday - 2 hours
Saturday - 4 hours
Sunday- 8 hours


