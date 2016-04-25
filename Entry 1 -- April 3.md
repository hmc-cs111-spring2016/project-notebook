# Design notebook for week ending April 3, 2016

## Last week's critique

First of all, I'm happy Joshua seems excited about my idea. I enjoy strategy games so much that I sometimes worry this idea
might not be interesting to the average gamer/coder. It also seems, like me, he has not seen or heard of anything else quite
like this -- wahoo for potentially novel ideas!

In reference to the potential pitfalls and rabbit holes for the project, I agree. This is a huge project that will never be
"complete" by the end of the semester. My goal is to have a fully functioning language where I can easily extend new API
behavior in the future. For instance, even if I choose to add a new type of laser in the future that has function calls
like `chargeLaser()` and `dischargeLaser()`, this should (from an implementation perspective) be just adding a new data type
to the abstract syntax tree that then gets interpreted when encountered. The actual aspects of the language should be
relatively consistent (i.e. math, control flow, etc.) since all ship functionality is essentially just a function calls that
generates a piece of the AST.

While I agree I could potentially spend a lot of time on graphics, GUI, and networking, this is an advantage of using Scala
and Java. I have a lot of Processing experience and the bare bones visualization I'm imagining right now should not take
too much effort to create. In other words, all the GUI is essentially doing is recieving messages from the server which it
simply renders. Although may be to naive of a design for an industry level game (since malicious gamers can potentially start
sending fake or falsified packets), I am choosing to not address this concern for the time being. Additionally, I have a friend
designing many of the sprites for me so I will not need to spend time drawing pictures (since my pixel artwork stinks and
image design isn't part of my language design). Finally, since Java and Scala both execute on the JVM, I have already managed
to get a simple Java and Scala project to share serialized classes back and forth through a localhost connection on port 2016.
This means I have already overcome the biggest potential networking concern.

Although I have done a lot of parallelization code in Java previously, I have not done anything like this in Scala (even
though it seems to mostly use the Java standard library). Additionally, most of my threading experience has been simply
scaling up parallel tasks, not doing things like game loops which require time sensitive synchronization. Any feedback
my critiquer can provide about my parallelization choices will be gladly welcome!

Finally, I am happy to report I have already began addressing the last point by beginning my design work! Below I will share
some of the bare bones API functions I am envisioning as well as a sample program I wrote. Any feedback on these is very
important!

###### How, specifically, did the feedback help improve the project?
Since this was the first critique, there wasn't much feedback to provide thus far. The most useful part of this feedback
was to confirm my idea was solid and there were no major oversights in the inital planning. I imagine feedback will
become more useful in the coming weeks.

###### Did the feedback point out or offer something you hadn't considered? Did it help you make a design decision?
Again, since this was the first critique, there wasn't much feedback to provide thus far. Thus no new design decisions
were made from it. Rather, it helped me reaffirm my current trajectory is solid.


###### Was it helpful in addressing the most pressing issues in your project?
Again, since this was the first critique, there wasn't much feedback to provide thus far. There were not any pressing issues
I was stuck with (yet).


###### How will you incorporate the feedback into your work? Will you change something about the design, implementation,
or evaluation as a result?
This was mostly discussed above. Since Joshua seemed to approve of my current trajectory, I will stay the current course I
am planning.


## Description

Since I did a wide range of tasks and research this week, I will provide a description in bullet point form.

   * I decided to split my original repo into two repos. The server is now stored [here](https://github.com/afishberg/binary-star-alpha-server) and the client is now stored [here](https://github.com/afishberg/binary-star-alpha-client). This seemed like the sensible thing to do since they are different programs that need to be compiled and run seperately. Furthermore, they aren't even written in the same language.
   * I was originally concerned with how to build my programs after downloading the repos. Although the Scala code is mostly just standard library code, the client needs the Processing JAR file. Although I had never started a Gradle project myself (i.e. I have built someone elses Gradle projects before and worked with Android), this week I spent a lot of time familiarizing myself with Gradle project design. Gradle projects are awesome since you can supply dependencies to be fetch from online Maven repos at compile time (i.e. Processing JAR). Additionally, I can push something called the Gradle Wrapper (gradlew) to my repo so people can build my code natively without needing to install Gradle.
   * I set up my IDEs. Building using Gradle means overriding certain aspects IDEs like Eclipse use by default. After struggling to get my Gradle build files to work in Eclipse and ScalaIDE (although they can work), I decided to try IntelliJ. Although I had never used IntelliJ prior, I need to say I am very impressed with its feel and production environment. I am normally a hardcore Eclipse user, but I may actually perfer IntelliJ in the future (we'll see how this project goes).
   * I designed a sample program and though about some basic API calls.
   * I began researching parser options in Scala (I might need to write one from scratch for all the functionality I need... but I would obviously perfer not to...)

## Questions

**What is the most pressing issue for your project? What design decision do
you need to make, what implementation issue are you trying to solve, or how
are you evaluating your design and implementation?**

I need to begin designing the AST for the language and writing the initial steps of the parser. To do this, I finish my research
on Scala parser options and begin implementing the basic data types.

**What questions do you have for your critique partners? How can they best help
you?**

   * What do you think of my sample program? How could I improve the language?
   * How should I name globals? Do we like the required `g_`?
   * Should I call API functions from a `class` like structure with a `.` operator? Or should I leave it as is? How do I handle function name collisions then? Should this just be an error?
   * Since the message classes sent between the server and client need to be both in the server and client project, how should I include them in both repos? Right now I am just copy/pasting the classes between the two repos. This seems dirty.
   * How should I commit asset (i.e. pictures/sounds/etc)? Should these go in the repo?

**How much time did you spend on the project this week? If you're working in a
team, how did you share the labor?**

I spend about 11 hours working this week. A lot of this was messing with Gradle builds and IDE as well as research.
