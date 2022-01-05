---
layout: default
---

Java Matrix Benchmark
{: style="font-size: 300%;font-family: Times New Roman, Times, serif;text-align: center;font-weight: bold;"}

Java Matrix Benchmark (JMatBench) is a tool for evaluating Java  [linear algebra](http://en.wikipedia.org/wiki/Linear_algebra) libraries for speed, stability, and memory usage.  This tool can be used by users to select the best library for their application and by developers for identifying bugs and weaknesses.  Its goal is to help improve the state of high performance computing on the Java platform.

JMatBench rigorously tests performance across a wide range of matrix sizes and linear algebra operations.  The runtime performance benchmark measures how fast each library can run under optimal conditions.  It generates accurate results by taking in account JavaVM runtime optimizations, dynamically adjusts to the platform it is run on, and uses well established good benchmarking techniques.  The stability benchmark evaluates several standard linear algebra operations for their accuracy, sensitivity, and ability to handle overflow/underflow conditions.  Memory benchmark determines the minimum amount of memory required to perform different tasks.  For more information see the [approach]({{site.baseurl}}/manual/Approach) page.


Runtime Performance Results:
{: style="font-weight: bold;"}

| Date       | Speed   | Num Threads | Bits | Year Made | Processor/Link | 
|------------|---------|-------------|------|-----------|----------------|
| *2021.12*  | 3.3 GHz | 6           | 64   | 2020      | [Xeon]({{site.baseurl}}/runtime/2021_12_Xeon1250/) |
| 2018.04    | Cloud   | 4           | 64   | 2018      | [Xeon]({{site.baseurl}}/runtime/2018_04_XeonQuad/) | 
| 2019.02    | 3.4 GHz | 4           | 64   | 2012      | [Core i5-3570K]({{site.baseurl}}/runtime/2019_02_i53570/) | 
| 2019.02    | 1.4 GHz | 4           | 64   | 2018      | [Raspberry Pi 3B+]({{site.baseurl}}/runtime/2019_02_RPI3BP/) | 

Memory Usage Results:
{: style="font-weight: bold;"}

| Date      | JavaVM    | OS            | Link                    |
|-----------|-----------|---------------|-------------------------|
| *2013.10* | 1.7.0_17  | Linux Mint 14 | [2013-10]({{site.baseurl}}/memory/2013_10/) |

Stability Results:
{: style="font-weight: bold;"}

| Date      | JavaVM    | OS            |           Link             |
|-----------|-----------|---------------|----------------------------|
| *2013.10* | 1.7.0_17  | Linux Mint 14 | [2013-10]({{site.baseurl}}/stability/2013_10/) |

For older benchmark results see [History Of Results]({{site.baseurl}}/manual/HistoryOfResults/).

<h2>Source Code</h2>{:.center }
[https://github.com/lessthanoptimal/Java-Matrix-Benchmark](https://github.com/lessthanoptimal/Java-Matrix-Benchmark)
{: .center }

Understanding the benchmark:

* [Runtime Performance]({{site.baseurl}}/manual/DescriptionRuntime)
* [Memory]({{site.baseurl}}/manual/DescriptionMemory)
* [Stability]({{site.baseurl}}/manual/DescriptionStability)

The [manual]({{site.baseurl}}/manual/Software) provides instructions on how to use the benchmark.
To understand how the runtime performance is benchmarked see the [methodology page]({{site.baseurl}}/manual/MethodologyRuntimeBenchmark).

# Tested Libraries

* [Colt](http://dsd.lbl.gov/~hoschek/colt/)
* [Commons Math](http://commons.apache.org/math/userguide/linear.html)
* [Efficient Java Matrix Library](http://ejml.org/) (EJML)
* [Jama](http://math.nist.gov/javanumerics/jama/)
* [jblas](http://jblas.org/)
* [JScience](http://jscience.org/) (Older benchmarks only)
* [Matrix Toolkit Java ](https://github.com/fommil/matrix-toolkits-java)(MTJ)
* [OjAlgo](http://ojalgo.org/)
* [Parallel Colt](http://sites.google.com/site/piotrwendykier/software/parallelcolt)
* [Universal Java Matrix Package](http://www.ujmp.org/) (UJMP) 
* [Elegant Linear Algebra for Java](http://la4j.org/) (la4j)

# Works in Process

* [ND4J](https://deeplearning4j.konduit.ai/nd4j/tutorials) working through documentation and missing features issues [TICKET](https://github.com/eclipse/deeplearning4j-examples/issues/1003)

If you wish to ask a question consider posting to the [discussion group](http://groups.google.com/group/java-matrix-benchmark-discuss), or if it is specific to a particular wikipage post a comment on the wikipage itself.  
