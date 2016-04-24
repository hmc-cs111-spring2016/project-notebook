# Design notebook for week ending April 24, 2016

## Last week's critique

My critique from last week was generally positive. I got a lot of good feedback on writing the tasting notes parser, especially in the context of language design. Daniel recommended I include more error messages in my parser, since the tasting notes need to be in a very specific format. Specific error checking would help the user of my DSL to understand how they could alter their input to be accepted by the parser. I'm not sure whether I want to focus on this with the time left in the project, though it seems feasible. It certainly makes sense to me. One alternate solution to this could be to write a very quick GUI with predefined fields which would automatically control the format for user input.

I also got the feedback that I should try to find a way to pass the JSON object directly to my Javascript visualization code. I think that there are ways to do this, primarily by passing data through my server connection. I know that I could use something like [Flask](http://stackoverflow.com/questions/33788341/passing-data-to-d3-after-preprocessing-in-a-flask-app) but that wasn't really my primary focus in the project. It's definitely a more efficient solution for the future, but I'm happy that I have something working for this project. 

The last bit of feedback I received was a suggestion for more visualizations. Daniel said that he thought it would be interesting to view flavors in the order that they were tasted, which got me thinking about how some flavors could be represented in a type of graph. Each line could correspond to a different flavor and the axes would be time versus intensity. I'm not sure how feasible this is in the current time scope, but it could be an interesting visualization to extend the project (see Description section for more comments on that). 

## Description

This week I learned a lot more about d3 visualizations and put a lot of thought into how my project could be extensible for future developers. Since I had the entire stack of the project working, my goal was to produce the visualization of one wine, including a display of all text elements and graphical elements. I experimented with pie charts for flavor display.

While trying to get this working, I was also talking with my critique partners and Prof Ben about how I could present a somewhat completed project. Prof Ben mentioned that I could think about ways to design both the language and the visualization code to make it accessible for developers. I struggled for a while about how this could best be done. There are a lot of ways to tackle this problem. 

One approach would be to alter the parser to process different formats of data objects in the tasting notes. Instead of including checks for a particular attribute name ("Flavor", "Aroma", etc), I would include AttrList data types or TextDefault data types. The parser already has most of this functionality but the user would be able to define the elements in the tasting notes corresponding to that format. The default format of each tasting note attribute would be a text block and attributes could be classified differently based on user arguments or more specification in the tasting notes text.

I also considered expanding the parser based on specifically named attributes and limiting the attributes that could be visualized beyond showing a block of text. However, I wanted to make something more extensible and thought it could be done most efficiently by declaring general types (lists, text boxes, etc) for the parsing language and letting users incorporate their own visualizations. For the purposes of the project and the scope of the demonstration, I decided to focus specifically on lists of attributes and notes in clumps of text.

If I have time before my project presentation, it seems like the most efficient way to determine the behavior of each tasting notes attribute is to include a config file or dictionary. It would store the names of standard attributes parsed differently than the default. For example, I could write tasting notes with a "Flavor" attribute and the dictionary would say that "Flavor" is parsed as a list by default. That would affect the final JSON object and the visualizations which could work with that attribute.

This dictionary feature would also give users the option to override the default options. For example, if the default type for "Flavor" was a list of attributes but I wanted to parse and visualize "Flavor" as a block of text, I could override this from the command line by entering somethign like `python run-server.py -v "Flavor" Text` where the arguments after the `-v` flag are the attribute to override and the type to which it is converted in the parser.

On the visualization side of the project, I tried to think about ways that developers could easily add new visualization behavior. I think that by providing some default types in the language, users could predict the JSON format of each attribute and write visualizations based on that expected data. Currently, it is easy to add new visualizations to the system. Each tasting notes attribute is extracted from the JSON object and given as input to a d3 visualization method. The user has to specify the visualization function corresponding to each attribute, though the default is to [append text to the SVG element](https://www.dashingd3js.com/svg-text-element). Ideally, the attributes would all have default visualizations depending on their type, but this works for the time being. If a developer wanted to write their own visualizations, they would need to add a Javascript file using d3 to the visualizations directory of the project and then specify the attribute they want to visualize in the `index.html` file.

Most of my time this week has gone into learning about d3 and thinking of ways to make my system more extensible and accessible to developers. I will present my DSL to the class on Wednesday. At the very least, I will be able to show them a text file containing wine notes and then produce a d3 visualization of those notes. Depending on what I can get working by then, I could have a more extensible and user-friendly system.


## Questions

**What is the most pressing issue for your project? What design decision do
you need to make, what implementation issue are you trying to solve, or how
are you evaluating your design and implementation?**

I don't think I have a lot of issues with the project, I just have way too many things I would like to implement and not enough time. I initially chose a project with a reasonable scope but it got twisted into a much larger project once I met a few roadblocks along the development process. My biggest decisions for the project still revolve around how to make it as user- and developer-friendly as possible. I have been struggling to resolve these two audiences while also considering the language design components of the project (and the class). I want to focus on a nice user experience that doesn't require precise tasting note formatting. I also want users to be able to customize their visualizations, compare different wines, and see tasting notes for a given wine. A lot of these features are not related to language design and would take a while to implement. In the context of this class, I have prioritized the developer experience because it involves the most significant language design questions. Though this is not my preference and the project has not turned out like I initially envisioned, I think that it involves a lot of interesting issues and unique challenges. My goal for the end of the project is to present a coherent demo to the class and share the lessons that I have learned about making a DSL for both developers and users.

**How much time did you spend on the project this week? If you're working in a
team, how did you share the labor?**

I spent upwards of 10 hours on the project this week, including time in class and writing this notebook entry.
