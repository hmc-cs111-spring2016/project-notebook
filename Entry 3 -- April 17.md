# Design notebook for week ending April 17, 2016

## Last week's critique

While it's not necessarily obvious, I actually could incorporate an AST into my project to improve perfromance. As of right now, each kernel handles only one opertaion. Ideally, if you had an expression like `d = a + b - c;`, the plus and minus operations would be handled at the same time by a single kernel. This would save some overhead from kernel launches and you wouldn't have to save the result of `a + b` only to have to load it again when computing `(a + b) - c`. However, I don't really have the time to implement a system like this and I think it's already been done before in [this paper](https://pdfs.semanticscholar.org/5849/3baae32440fec5d46bdc1bfa156cda19d14e.pdf) under very similar circumstances. However, the main overhead in my code right now is probably copying data back and forth between the CPU and GPU. In the next update, I plan to use the framework I've been working on to completely eliminate `std::vector` from my code and get lazy data transfer working (i.e. only copy data back to the CPU when it's needed, otherwise just keep it on the GPU).

Language testing is also an interesting concern. While it wouldn't be a perfect solution, I could use OpenCL to run my code single-threaded on the CPU, which could get close to serial performance. I could also try to revive my original serial code, but that would only use the operations available at that time. Finally, I could just write some simple code that only tests a few operations, but does a fair performance test to see how they compare. This will probably be the best option, even if it isn't comprehensive.

## Description

**TODO:** Fill in this part with information about your work this week:
important design decisions, changes to previous decisions, open questions,
exciting milestones, preliminary results, etc. Feel free to include images
(e.g., a sketch of the design or a screenshot of a running program), links to
code, and any other resources that you think will help clearly convey your
design process.

While I didn't add any operations to my code, I did get OpenCL fully working and added a lot of stuff that will be helpful in implementing future features. My code now automatically selects an available GPU for computation and can copy data back and forth between it and the CPU. It is also ready to use the CPU in cases where that would be faster than using the GPU. As I hinted at earlier, I've also written methods that will be helpful for transitioning away from `std::vector` towards OpenCL data structures. Finally, I also simplified the type to string conversion method a bit (no more object madness).

Here is some preliminary performance data from running the example program with 10,000,000 element Vectors. Note that these were all run within OpenCL (so the serial time still suffers from overhead). Also, iGPU is my integrated GPU and dGPU is my dedicated / discrete GPU.

| Device        | Runtime |
|---------------|---------|
| CPU, serial   | 11.033s |
| CPU, threaded | 8.225s  |
| iGPU          | 10.849s |
| dGPU          | 12.745s |

## Questions

I still need to get casting, non-elementwise operations, and lazy memory copies working. While I could do more, this will probably turn out to be more work than even I expect.

I'm not sure I really have many design decisions to deal with right now. I've already talked about method names and casting, so you don't have to worry about those. I am considering eliminating return values in all unnecessary cases, though (e.g. +=, ++, --, etc.). Do you think this would be too restrictive or would it help programmers overall when working with objects?

Also, perhaps you could help me test how easy it is to get my code working since you won't have many questions to answer this week. Just try downloading the source code [here](https://github.com/JoshuaLandgraf/ParallelVector) and see if (1) it compiles on your computer using the command in the Readme*, (2) the executable runs without crashing, and (3) if every other line matches the line below it (i.e. the computed values are correct). If you are on an older machine, you may want to decrease the size of the Vectors in the example program to something less than 10,000,000 so that it runs a little faster. Since this is a program that uses your GPU, it's also good to save your work beforehand just in case the GPU crashes.

* If you're on Windows, this command probably won't work. Instead,  you could try following the instructions on the bottom of [this website](https://www.fixstars.com/en/opencl/book/OpenCLProgrammingBook/first-opencl-program/) if you have Visual C++ and see how that works out.

I spent somewhere around 5 hours this week on my DSL outside of class. I didn't have as much free time due to my family being here and thesis, but made a lot of progress in the time given.
