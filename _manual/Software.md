---
permalink: "/manual/Software/"
layout: page
title:  "Software Manual"
---

The following is instructions for building and running JMatBench in the most straight forward way.  Apache [ant](http://ant.apache.org/) is used to compile and build a java commandline application "benchmark_app", which can then be used to run the most common tools in a simple way. 

# Building

To build JMatBench first the source code must be acquired from either the git repository or from a prebuild package at UPDATETHIS.  An ant script is provided for building "benchmark_app".  Benchmark_app is an application that provides an easy to use command line interface for running various benchmarking tools.  Running the ant script requires that both ant and java be installed.  Once that has been done simply run the "app" script inside of build.xml and it will compile the benchmark and put benchmark_app.jar inside of the "build/jar" directory.  

The following is a command line example in linux:
<pre>
$ ant
Buildfile: build.xml

compile:
    [mkdir] Created dir: jmatbench/build/classes
    [javac] Compiling 129 source files to jmatbench/build/classes
    [javac] Note: jmatbench/src/jmbench/impl/runtime/UjmpAlgorithmFactory.java uses or overrides a deprecated API.
    [javac] Note: Recompile with -Xlint:deprecation for details.
    [javac] Note: Some input files use unchecked or unsafe operations.
    [javac] Note: Recompile with -Xlint:unchecked for details.

jar:
    [mkdir] Created dir: jmatbench/build/jar
      [jar] Building jar: jmatbench/build/jar/jmatbench.jar

app:
      [jar] Building jar: jmatbench/build/jar/benchmark_app.jar

BUILD SUCCESSFUL
Total time: 7 seconds
</pre>

# Running the Benchmarks

The easiest way to run different benchmark tools is using the "benchmark_app.jar", which provides a command-line interface. A list of tools that it can invoke is provided below:

* Runtime Benchmark
* Stability Benchmark
* Memory Benchmark
* Runtime Check
* Plot Runtime Results
* Display Stability Results

To get instruction on how to run these tools simply invoke benchmark_app with no arguments.  In linux make sure you are inside of the main jmatbench directory then type:

<pre>
$ java -jar build/jar/benchmark_app.jar

A tool must be specified:

  java -jar benchmark_app.jar < tool >

Where tool is one of the following keywords: 
  stability          Runs the stability benchmark.
  runtime            Runs the runtime benchmark.
  memory             Runs the memory benchmark.
  checkRuntime       Outputs the runtime sanity check results.
  plotRuntime        Generates plots from runtime results.
  displayStability   Prints out tables showing stability results.

For example to run the runtime benchmark type:
  java -jar benchmark_app.jar runtime

Additional help on commandline arguments for each tool can be obtained by typing:
  java -jar tool < help >
</pre>

## Results Directory

By default results for all new tests are stored in a new directory created in the results directory.  The name of the directory will be the time in milliseconds when the test was started.  This way old results are not accidentally overwritten.


## Runtime Performance Benchmark

It can take several days for the runtime benchmark to run to completion on most systems using the default configuration.  How long it takes is some what independent of the computer it is run on.  Command line arguments are provided for modifying common parameters such as the range of matrix size it tests.

An optional parameter is to turn on a runtime sanity check.  This ensure that each library is in fact performing the expected operation and generating reasonably accurate results.  The down side is that running with this feature turned on increases the memory requirements and slows down the benchmark, which is why it has been turned off by default.

An alternative way to configure the runtime benchmark is by editing a config.xml file and telling the benchmark to use it.  If running the same benchmark on multiple computers this will ensure that the same random seed and other options are identical.  It is also much more flexible than the command-line arguments.
  
Instead of running the whole rigorous benchmark a quick and dirty trial can be run by invoking --Quick.  Results is be much less accurate (especially for small matrices) but the whole benchmark can be run in a day or so.

By default the benchmark will dynamically determine the amount of memory to allocate.  This can slow the benchmark down and can cause problems if it starts using swap space.  Therefor it is recommended that a fixed amount of memory is allocated.  This can be done using --FixedMemory.

To run a quick benchmark on larger matrices with a fixed memory of 7000 MB:
<pre>
java -jar build/jar/benchmark_app.jar runtime --Quick --Size=500:10000 --FixedMemory=7000
</pre>

## Stability Benchmark

Stability benchmark measures how prone each library is to errors under different conditions in different operations.  On 2.4 GHz x86 processor this takes about 2 days.

# Memory Benchmark

Estimates the amount of memory each library requires to perform different operations.

## Show Runtime Check Results

If sanity checks were turned on this will display any errors that occurred.

## Plot Runtime Results

When invoked without any parameters it will show relative runtime results and create a plots directory containing absolute and relative plots from the last runtime results created.  It is possible to specify a results directory for it to plot.

## Display Stability Results

Creates a table (html or plain text) that shows the stability benchmark results.
