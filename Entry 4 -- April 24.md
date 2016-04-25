# Design notebook for week ending April 24, 2016

## Last week's critique

I'll address last week's critique in the order in which it was given.

I like the Readme suggestion and have combined my two Readmes. Unfortunately it's still pretty empty, but I hope to add more for the final submission.

Thanks for trying to get the code to compile. I actually have Windows 8.1 installed on an external hard drive, so I can see if I can get it working on that.

Unfortunately I just don't have the time or expertise to implement AST optimizations in C++. I'm glad you don't think this is necessary, though. It would have made an already difficult project impossible.

Overhead is really worth accounting for when doing perfromance testing. I'd really like to pit my DSL against ideal serial code and see what happens, but I may not have the time to do so. Also, fun fact: on OS X, you don't need a VM to limit the number of CPUs availalble. You can use the preferences pane of the Instruments tool (from Xcode) to both adjust the number of CPUs available and enable or disable HyperThreading. However, if the VM had a CPU dedicated to itself, there may be less overhead from system processes, but then you get overhead from using the VM.

I ran my code on my Mid 2012 Retina MacBook Pro which has an integrated Intel HD Graphics 4000 GPU and dedicated Nvidia 650M GPU. The CPU itself has 4 cores with hyperthreading.

I'm glad you are ok with eliminating return values. I've found them to make code too confusing.

I agree with adding a Makefile. I can at least add support for my system and maybe Linux as well. However, I don't know how well Windows supports Makefiles and how easy it would be to get them working for Windows. I think I'll skip making the `/src/` and `/bin/` folders, though. They might make more sense for a more complicated build, but this project keeps things about as simple as they can get.

I like your suggestion. I've been working on a revamped testing program that will test more features and have better testing granularity. Assert statements would definitely help.

This is a good suggestion as well. I can add a reference to where I got `cl.hpp` in the Readme.

I liked this suggestion as well. If I'm going to use C++11 stuff, I might as well include type-safe null pointers.

Again, I think this is better advice for larger projects. I'm trying to keep stuff simple and easy for the end user. Ideally I'd like everything to fit into one header file, but it's probably for the best that I don't try to integrate `cl.hpp` into my own header file.

I'll see what I can do here as well. Unfortunately time is a big concern, so making comments may get put off in favor of adding documentation, example programs, and other good stuff.

## Description

This week I made some huge changes (some of which were in response to some massive hurdles C++ threw my way). Initially, I tried to get my code to only copy data between the CPU and OpenCL when necessary. However, after exploring this for a long time, I found that this is very difficult to do well. You see, when overloading an operator that involves two objects in C++, you can't modify the other object. This means that if the other object had data on the CPU, I could copy it into OpenCL, but I couldn't save the reference to it in the original object. This means that the data might have the be copied over again in the next computation and the computation after that and so on. While I might have been able to work around this through a lot of extra work and some advanced hacking, I reluctantly embraced C++'s requirements by guaranteeing that the data in OpenCL was always up-to-date. This way I would never have to worry about having to copy it over and they throw away the copied data. However, this will change how users work with the DSL significantly. While they can still work with large amounts of CPU data, the data will need to be transfered all at once for things to be efficient. So, work involving lots of changes on the CPU should be done in `std::vector` and then copied en masse into the Vector. I did implement differential changes so that it is possible to "efficiently" change a few values from the CPU if neccesary. However, this will be extremely slow if done on a large scale.

## Questions

Unfortunately, due to the massive changes mentioned earlier, I didn't have much time to implement other features that are very important to the DSL and will need to get those done in the next week. I don't really have many critique questions as there aren't really any design questions left.

I've spent arount 7-8 hours already this week and will be working a lot more tonight to get the aforementioned changes full working and to prepare for the presentation tomorrow.
