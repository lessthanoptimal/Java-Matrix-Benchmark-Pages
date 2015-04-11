---
permalink: "/manual/Approach/"
layout: page
title:  "Benchmarking Approach"
---

Java Matrix Benchmark is a tool for evaluating different Java matrix libraries for speed and stability.  This is accomplished through two benchmarks, runtime and stability.  The runtime benchmark attempts to measure the best possible runtime performance for different linear algebra operations in each library.  The stability benchmark tests different common linear algebra solvers for stability and accuracy using general purpose tests.

Performance results are presented using relative runtime plots and tables.  Relative runtime plots show how fast each library performs relative to other libraries.  They do NOT show the FLOPS for each library.  Stability benchmarks are presented using tables.  These tables show either the accuracy or the breaking point of each algorithm as well as errors that occurred during testing.

The following pages provide more information on the specific benchmarks.  Links to the methodology go into detail about how the results are computed and why there were computed that way.  Links to the descriptions describe what was benchmarked and how to understand the results.

Methodology:

* [Methodology for the Runtime Benchmark]({{site.baseurl}}/manual/MethodologyRuntimeBenchmark/)

Description:

* [Runtime Benchmark]({{site.baseurl}}/manual/DescriptionRuntime)
* [Memory Benchmark]({{site.baseurl}}/manual/DescriptionMemory)
* [Stability Benchmark]({{site.baseurl}}/manual/DescriptionStability)

# Benchmark Bias

All benchmarks have a certain bias that will favor one approach over another.  The runtime performance benchmark is intentionally biased to favor libraries that provide greater control of memory management and highlights poor performance on small matrices.  These are two areas that around the end of 2009 many Java based libraries were weak in and would hamper high performance computing in certain applications.  In many cases libraries would only require small modifications to have significant performance gains.

The runtime performance benchmark is designed to measure the best possible performance of each library.  It provides a realistic benchmark when a programmer knows how to effectively use a library and has take steps to optimize performance.  This might involve preallocating memory and calling the appropriate decomposition for the matrix type.

There are some real-world situations that would not be accurately represented by this benchmark.  Writing efficient code can be time consuming and is often not necessary.  If all one wants to do is to prove something is possible, why spend the extra time optimizing the code?  A significant amount of development time can be saved by not learning all the intricate details of a library, calling generic operations, and by managing memory in an ad hoc manor.  In these situations the practical performance merits of a library might be different than what is suggested in this benchmark.  However, this benchmark should still provide some insight.

It should also be mentioned that the developer of this benchmark is also the developer of Efficient Java Matrix Library (EJML).

# Acknowledgements

Thanks to the comments and feedback from several people the quality of this benchmark been improved.

In no particular order:

* Anders Peterson  
* Luc Maisonobe    
* Holger Arndt     
* Mikio Braun      
* Frode Carlsen    
* Piotr Wendykier 
