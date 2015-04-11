---
layout: default
---

# Java Matrix Benchmark

Java Matrix Benchmark (JMatBench) is a tool for evaluating Java  [linear algebra](http://en.wikipedia.org/wiki/Linear_algebra) libraries for speed, stability, and memory usage.  This tool can be used by users to select the best library for their application and by developers for identifying bugs and weaknesses.  Its goal is to help improve the state of high performance computing on the Java platform.

JMatBench rigorously tests performance across a wide range of matrix sizes and linear algebra operations.  The runtime performance benchmark measures how fast each library can run under optimal conditions.  It generates accurate results by taking in account JavaVM runtime optimizations, dynamically adjusts to the platform it is run on, and uses well established good benchmarking techniques.  The stability benchmark evaluates several standard linear algebra operations for their accuracy, sensitivity, and ability to handle overflow/underflow conditions.  Memory benchmark determines the minimum amount of memory required to perform different tasks.  For more information see the [approach](/manual/Approach) page.


Understanding benchmark pages:

* [Runtime Performance](/manual/DescriptionRuntime)
* [Memory](/manaul/DescriptionMemory)
* [Stability](/manual/DescriptionStability)

The [manual](manual) provides instructions on how to use the benchmark.
To understand how the runtime performance is benchmarked see the [methodology page](/manual/MethodologyRuntimeBenchmark).

Runtime Performance Results:

| Date       | Speed   | Num Threads | Bits | Year Made | Processor/Link | 
|------------|---------|-------------|------|-----------|----------------|
| *2013.10*  | 3.4 GHz | 4 (8)       | 64   | 2011      | [Core i7-2600](/runtime/2013_10_Corei7v2600/) | 
| 2011.01.18 | 720 MHz | 1           | 32   | 2010      | [Arm Cortex-A8](/runtime/2011_01_ArmA8/) |

Memory Usage Results:

| Date      | JavaVM    | OS            | Link                    |
|-----------|-----------|---------------|-------------------------|
| *2013.10* | 1.7.0_17  | Linux Mint 14 | [2013-10](/memory/2013_10) |

Stability Results:

| Date      | JavaVM    | OS            |           Link             |
|-----------|-----------|---------------|----------------------------|
| *2013.10* | 1.7.0_17  | Linux Mint 14 | [2013-13](/stability/2013_10) |

For older benchmark results see [History Of Results](/results/HistoryOfResults/).

Tested Libraries: 

* [Colt](http://dsd.lbl.gov/~hoschek/colt/)
* [Commons Math](http://commons.apache.org/math/userguide/linear.html)
* [Efficient Java Matrix Library](http://code.google.com/p/efficient-java-matrix-library) (EJML)
* [Jama](http://math.nist.gov/javanumerics/jama/)
* [jblas](http://jblas.org/)
* [JScience](http://jscience.org/) (Older benchmarks only)
* [Matrix Toolkit Java ](https://github.com/fommil/matrix-toolkits-java)(MTJ)
* [OjAlgo](http://ojalgo.org/)
* [Parallel Colt](http://sites.google.com/site/piotrwendykier/software/parallelcolt)
* [Universal Java Matrix Package](http://www.ujmp.org/) (UJMP) 
* [Elegant Linear Algebra for Java](http://code.google.com/p/la4j/) (la4j)

If you wish to ask a question consider posting to the [discussion group](http://groups.google.com/group/java-matrix-benchmark-discuss), or if it is specific to a particular wikipage post a comment on the wikipage itself.  
