# Design notebook for week ending April 3, 2016

## Last week's critique

This DSL will probably fall somewhere into the realm of "you can use it as a full language, but you really wouldn't want to". Like OpenMP, it aims at making parallelizing highly repeated operations easier. In order to do this in a reasonable timeframe and get decent speedups across multiple platforms, my DSL will focus on vector-level parallelism. In OpenMP, you would have to loop over every index in your dataset and specificy how to do the operation for that index. With my DSL, the loop (and extra OpenMP syntax) is elimiated entirely. Ideally, you'd use a ParallelVector like you would a `std::vector` in your code. But, when it comes time to loop over every index in the `std::vector` like you would have to do in OpenMP, you can just pretend your vector is a single variable and forget about all the parallelism stuff. While certain operations (like advanced logic) won't be possible or as easy in OpenMP, I hope for my DSL to be easier to use (hence trying to fit it into a header file) and to make up for potential shortfalls in performance by being able to use GPUs for large computations. In HPC, we actually had an entire homework assignment or two on vector-level parallelism and what you can do with it despite its simplicity. The example code I made this week might give you a better idea of how powerful my DSL could be (instead of 10 elements in the vectors, imagine thousands, and instead of running in serial, imagine the code running on a gaming or HPC class GPU). If you want a concrete use case, this kind of parallelism could work really well with computing the propogation of sound in 1D where the data is linear and the conmputations don't involve crazy logic except perhaps at the edges. It could also work for problems where brute forcing is required, perhaps like password cracking or cracking encryption keys (64 bits or under, so don't get your hopes up).

The good news with performance is that elementwise operations are probably the most ideal parallel problem you could get. So, that shouldn't be hard at all. Reductions, on the other hand, are a different story. Any decent parallel language / library will use really advanced optimizations for this kind of stuff. So, for the sake of my own sanity and my DSL's performance, it would be for the best to either use a library that already has good reduction implementations or go with a parallel language where there is plenty of good source code available.

To critique the critique as the TODO suggests, the most important issue raised is ease of use. As much as I'd like this DSL to become popular and powerful, the biggest impact I can have it making it dead easy to do powerful parallel operations. I don't know if vector parallelism exactly falls under the description of making "people specify exactly how everything is parallelized." Usually with these kinds of operations, you're writing a for loop that could easily be parallelized by OpenMP or you aren't. If you are, then my DSL could potentially improve your performance over OpenMP, be easier to get working, and result in simpler, more compact code.

## Description

This week, I laid the framework for a lot of future progress in the DSL. I have an overloaded operator for practically every integer and logic operation that there is in C++, which is really scary actually (because there are a ton to cover). However, this did help me work on code organization and I was able to brush-up my knowledge of C++ namespaces, classs, constructors, and operator overloading. Unfortunately, I didn't get to reductions yet, so there will need to be more methods to come.

I also didn't get to start on actually getting stuff to run in parallel for two reasons. First of all, it's good to have a serial implementation to test against (though this wasn't my main reason). The most important reason is that there are a lot of parallel languages / libraries that I could build on, but they all have very important downsides. While I have an idea what would be best, I'd like a confirmation by someone who's not buried in parallelism like I am. The three major choices for implementation languages, their advantages, and their disadvantages are outlined below. Please let me know which you would feel is best.

## Questions

Kokkos
This was what I actually planned on using for this project. Kokkos is a library made by Sandia National Laboratories that, in theory, allows you to write a program once, and run it at practically full performance on a wide variety of parallel systems. This includes parallel CPUs and Nvidia GPUs at the moment. However, Kokkos is a nightmare to program in (the documentation is so bad that it's sometimes easier to go through the source code to figure things out) and it doesn't support AMD GPUs or other OpenCL devices (like some AMD CPUs are equipped with). Also, the build system is terrible and getting Kokkos up and running is no small feat. While the promises of great performance with a single implementation are enticing, I feel like the lack of device support, complex build system, and unfinished status of the library may hold many users back.

Thrust / Cub / and OpenMP
Thrust is actually a library kind of like what I want my DSL to be. However, some poor design decisions make it painful to look at and unfit for people who aren't really motivated to parallelize their code (does `int x = thrust::reduce(d_vec.begin(), d_vec.end(), 0, thrust::plus<int>());` look nice to you?). Thrust also forces users to manually move data between the CPU and GPU and suffers from bad performance if users don't make functors for all but the simplest of operations. However, with the help of Cub (another Nvidia GPU library), I might be able to shield users from most of that nonsense. I would then use OpenMP to target systems with parallel CPUs, but this would require two separate implementations for each function. It would also be limited to supporting the same devices Kokkos does, but wouldn't require an insane build system (Thrust was moved into the Cuda language, Cub is pretty straightforward to use, and OpenMP is probably much easier to install than Kokkos, especially given it's wide usage). So, compared to Kokkos, the implementation will have a lot of redundancy, but may ultimately be easier on users (especially if I could make it so you didn't have to install OpenMP to use an Nvidia GPU and vice versa).

OpenCL
OpenCL is a widely-supported language that practucally everything runs. It works on both AMD and Nvidia GPUs as well as Intel CPUs and at least some AMD CPUs / integrated GPUs. While you have to change up the include file between systems, many computers should already have good support for getting OpenCL working and the header files may already be installed on Macs with Xcode. However, OpenCL makes it hard to write and debug parallel programs because you have to load the source code and compile them at run time (this is probably required to support all the different systems out there as you don't know which will be running your code and can't compile for them all). I am also the least familiar with OpenCL of all the languages listed here, but it is pretty similar to Cuda (Nvidia's GPU programming language) and Kokkos in a lot of ways. So, I would only have to write each operation once, it would support practically everything, and it would prbably be the most accessible to users, but it will probably be a bit more difficult to write and debug than Kokkos and the performance won't be the greatest. Right now, I think OpenCL is probably the best bet, but let me know if you think otherwise.

Besides implementation languages / libraries, the other major design issue I'm struggling with right now is how to implement ternary operators / control flow into my DSL and how to make reductions as user-friendly as possible. Since C++ doesn't allow overloading of the ternary operator, I'd either have to hack together something that looks similar or just use an intuitive method name. I've listed some example syntaxes below if you want to help pick which looks best.

Ternary operator (pretend A, B, and C are regular variables)
```C++
A ? B : C        // hacked together, possibly destabilizing code
ternary(A,B,C)   // not hacked together, just a regular method
choose(A,B,C)
switch(A,B,C)
pick(A,B,C)
if(A).then(B).else(C)   // more hacks, but maybe not as many as the first
A.isTrue(B).isFalse(C)
B.if(A).otherwise(C)
B.if(A).else(C)
```

Reductions (note that 0 is the starting value; we would use 1 if multiplying)
```C++
scalar\_variable = vector\_variable.reduce(PV::plus, 0);   // PV is the ParallelVector namespace in case you're wondering
scalar\_variable = vector\_variable.reduce("+", 0);
scalar\_variable = vector\_variable.sum();
scalar\_variable = vector\_variable.sum(0);
scalar\_variable = 0; scalar\_variable += vector\_variable;   // not sure if this is actually possible, but it would be cool
```

Also, check out my latest code and examples [here](https://github.com/JoshuaLandgraf/ParallelVector). You can skim pretty much all of it. Just let me know how it looks. Does the organization look ok? Are the names reasonable? What changes would you like to see in the langauge itself? Also, most of the implementation details may / will change, so only critique it at a high-level.

I spent around 8 or 9 hours coding and writing stuff up outside of class (this does not include work on critiques for people in my group). I've also been thinking about language implementation details and how to save time in the long run, but this is not included in the time given either.
