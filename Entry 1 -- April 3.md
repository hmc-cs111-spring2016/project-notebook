# Design notebook for week ending April 3, 2016

## Last week's critique

The critique I received last week identified a technical difficulty as well as a word of caution in terms of the evaulation of my project. The technical difficulty was something we discussed in class, which concerned how I would address the issue of cutting and splicing audio files, as time stamps aren't exactly the most intuitive and easy way to do so. Aaron also suggested perhaps more specific standards of evaluation, which I agreed with, as well as erring on the side of caution for not recognizing logistical / abstract-level obstalces, which I also agreed with.

The feedback did help me improve my project, rather significantly, actually, by helping me come to a desgin decision. Because of the tricky issue of time stamps, I have followed Milo and Aaron's suggestion and have decided to focus on audio effects that change the entire audio file rather than specific ranges as a starter. Additionally, I have found a rather nice library to use that will help at least with handling time stamps later on. Considering that determining a final design for my DSL was the goal for this week according to my project plan, this critique was very helpful in addressing what I considered to be the most pressing issues in my project at the moment. 

## Description

As described above, I've come up with what I feel like is a pretty solid design plan as well as deciding on a host language and finding all necessary libraries to complete the DSL in said host language. The host language I ultimately decided on using is Python. For more details, check my [language design and implementation overview](https://github.com/williumchen/project/blob/master/documents/design_and_implementation.md). To reiterate, a few important design decisions and clarifying points for my project plan include: 

* Decide on developing volume and pitch commands that affect the entire audio file for prototype
* Syntax for both: "+/- volume/pitch (1-100)"
* Add more effects unit that will involve natural langauge syntax (more complicated effects like chorus effect)
* Stretch goals: extensible language that allows users to combine provided audio effects under a new name

## Questions

**What is the most pressing issue for your project? What design decision do
you need to make, what implementation issue are you trying to solve, or how
are you evaluating your design and implementation?**

The most pressing issue for the project now is getting started with the parsing, as I need to figure out how exactly parsing works with Python. I intend to use pyparsing, which is a parsing library in Python, and I have already looked at / written a few tutorial / example demonstrating how pyparsing works. Given that I have found a nice library (pydub) that has a lot of the basic functionality I need for semantics, the semantics for the prototype stage seems less important for now. A few questions regarding design are explained below. 

**What questions do you have for your critique partners? How can they best help
you?**

I had a question regarding development environment and control structures. Because Milo will be critiqueing me this week, I think this question is also relevant to the DSL you're working with as well.

The more I think about it, the more it seems like our DSLs are ultimately data interpretation languages. In my case, it reads in a series of commands (data), interprets the command, and applies it to some audio file. Because it seems strongly rooted in data interpretation, do you think there will be any opportunities for a control structure? I said no in my language design and implementation, but I'm not exactly sure. Also, I said there would be no development environment, because I was imagining you would run most of these commands via console; however, would it just be better to use a REPL interface? As of now, given the direction of the DSL, it seems the user will be writing a script that will be executed in console. 

**How much time did you spend on the project this week? If you're working in a
team, how did you share the labor?**

* 4 hours thinking about + writing up project plan details, including the langauge design and implementation
* 2 hours in studio/class time, researching and finding the libraries I will need
* 1 hour writing up an example parser using pyparsing
* 1 hours writing up this notebook
