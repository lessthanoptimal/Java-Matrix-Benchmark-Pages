---
permalink: "/manual/MethodologyRuntimeBenchmark/"
layout: page
title:  "Runtime Benchmark Methodology"
---

The runtime performance benchmark portion of Java Matrix Benchmark is designed to measure the best possible performance of each library in different linear algebra routines.  It provides a realistic benchmark in situations that the developer knows how to effectively use a linear algebra library and has take steps to optimize performance, such as predeclaring all memory.  

For sake of brevity, the following discussion is focused entirely on how to create an effective benchmark that measures best possible performance in optimized code.  There are other types of benchmarks that also have merit.  For example, a benchmark could measure less optimized code that might be common in research and development (R&D) environments and report the expected performance.

# Performance Benchmark

In principle benchmarking an algorithm is a simple task.  Just measure how long it takes to perform an operation and save the results.  For this project we wanted measure how fast various matrix libraries are at performing different linear algebra operations across a variety of matrix sizes.  A very simple benchmark for this project might go something like:

{% highlight java %}
public void performBenchmark( Libraries libraries ) {
    for( Libraries lib : libraries ) {
        for( Operation op : lib.getOperations() ) {
            for( int matrixSize = 0; matrixSize < maxSize; matrixSize++ ) {
                long timeBefore = System.currentTimeMillis();
                    
                op.performTest( matrixSize , numberOfTrials );
                    
                long timeAfter = System.currentTimeMillis();
                    
                saveResults( lib , op , matrixSize , timeAfter-timeBefore );
            }
        }
    }
}
{% endhighlight %}

This approach is very simple and for many application is good enough, when only a crude estimate is needed.  Unfortunately it will produce inaccurate results in practice because it fails to take in account several factors:

1. [Implicit System Assumptions](#1.1)
2. [Previous History Infuencing Future Results](#1.2)
  * Previously run libraries can degrade the performance on other libraries
  * Accidental overloading of library classes
  * Runtime optimization performed by the JavaVM
3. [Benchmark overhead.](#1.3)
4. [Repeatability](#1.4)
5. [Exception Handling](#1.5)


In the following subsection these issues are discussed and examples given to illustrate their importance.

  # [#1_Benchmark_Procedures Benchmark Procedures] 
  # [#2_Summary Summary] 

# 1 Benchmark Procedures

Measuring runtime performance accurately requires benchmark procedures that carefully take in account the reality of how modern computers operate. Desktop computers today are very complex and no single application has control over the entire system.  This complexity adds more unknowns and makes it more difficult to predict future performance based upon your past results, which is what a benchmark is used to do.  

Benchmarking in a Java environment is even more difficult than native code, because of the additional layers of abstraction from hardware.  Even a simple Java application will show more variability in its runtime than a simple application written in c.  Modern Java virtual machines (VM) will actually change the executed code at runtime.  Making benchmarking more difficult.  

## <a name="1.1"/>1.1 Implicit Assumptions

When writing a benchmark it is hard, if not impossible, to not make implicit assumptions about the system the benchmark is run on.  The most common culprit is the variable 'numberOfTrials' that determines the number of times a particular trial is run for a test.  If set too small then the performance cannot be accurate measured.  If set too high then it will take too long. For example, if a benchmark is written on a very fast computer then in might be too slow when used on an older computer.  Then years later when the same program is run on a new computer it might be too fast.

A benchmark running too fast leads to inaccurate results because there is always noise in how the execution time measured.  Mathematically this can be viewed as a random process:
{% highlight java %}
t_m = t_0 + N(t_0)
{% endhighlight %}
where 't_m' is the measured time, 't_0' is the true time, and N(t_0) is a noise process that may be dependent on when it is called.  This issue is exaggerated in Java due to the abstraction from the systems hardware, preventing very low level system calls from being called.

In Java a common function to measure time is System.currentTimeMillis() and it has an unspecified accuracy that is assumed to be within milliseconds.  In practice its accuracy is variable depending on which operations/hardware it is run on.  System.nanoTime() returns a more precise time measurement, but its documentation explicitly states that its accuracy is not defined, see the excerpt from its documentation below:
```
This method can only be used to measure elapsed time and is not related to any other.
This method provides nanosecond precision, but not necessarily nanosecond accuracy. 
```

One way to handle this problem is to simply increase the length of the benchmark until the noise is insignificant.  As previously mentioned, the number of trials that this requires is dependent on the system it is run on.  

In this benchmark matrices of different sizes are used as inputs.  In general, the larger the matrix is the longer it takes to process.  Determining the maximum matrix size has similar issues to determining the number of trials and is system dependent.

### 1.1.1 The Java Matrix Benchmark Approach

Java Matrix Benchmark dynamically determines the number of trials by estimating how long each operation takes.  Each time a new benchmark is run it first runs a small number of trials.  Then it estimates how long it takes to run each operation and increases the number of trials so that it should last for a minimum duration T<sub>min</sub>.  This process is repeated until it approximately runs for T<sub>min</sub> seconds.  At which point the trial stops and the number of operations per second is returned.

The size of the matrices that it processes is also dynamically determined at runtime.  This is done simply by having a maximum amount of time allowed for each test.  If that time is exceeded the test stops.  A maximum matrix size is also specified to prevent it from running for too long or beyond any interesting point.

Another factor that determines which system a benchmark is run on is the amount of memory allocated to Java.  A generous estimate is made for how much memory a process will require and only that amount of memory is allocated when a test instance is launched.  For a small matrix only a few megabytes might be allocated, but for large matrices over a gigabyte might be.  If an application fails to launch due to lack of memory then its memory allocation will be increased until it can run.  Thus without modifications this benchmark could be run on an embedded system with 100 megabytes of RAM or a desktop with 6 gigabytes.

A side effect of this approach is that the amount of time it takes for a complete benchmark to run is approximately system independent.  It would take about as long on a state of the art computer 10 years from now as it would on an old door stop collecting dust.

## <a name="1.2"/>1.2 Previous History Infuencing Future Results

A basic assumption made when measuring an algorithm's performance is that its current performance is independent of what was run before it.  If this assumption is broken then results become unpredictable or unreproducible on different systems. At a glance the sample code above wouldn't have this issue, since by the time new library is being evaluated the old library would have stopped and would no longer be running. Unfortunately this is not the case.  A few of the reasons why are listed below:

1. The previous library or operation could have left stray threads around.
2. The Java garbage collector might have not had run yet in the previous test and so it runs in the current one.
3. Two libraries might have different classes with the same name and classpath.
4. The virtual machine might have optimised itself for a different configuration and might be a suboptimal state for the current one.

1) With concurrent (multithreaded) algorithms becoming more and more popular as multiprocessor/multicore systems become mainstream problems associated with them will increase.  Writing good multithreaded is not easy and there are several common errors that can significantly effect other tests performed later on.  For example, a deadlock condition is when a thread is waiting for something to happen that never will happen.  The thread might never stop consuming computation and memory resources until it is killed by the application exiting.

2) Java applications have very little control over when the garbage collected might run (1).  They can request that the garbage collector run at a particular time, but they can not force it to run.  It is possible for one test to consume a lot of memory only to have the next test take a performance hit when the garbage collector runs.  How the garbage collector behaves is highly dependent on its own algorithm, which constantly changing with each new release of the JavaVM.

3) It is possible for two libraries to have a class with exactly the same class name and package but have different behavours.  If this happens and both libraries are tested inside the same application instance then Java will override one library's classes with the others at runtime silently making the benchmark's results invalid.  

While this is unlikely it is possible.  Some libraries are derived from other libraries, share a common ancestry, or maybe the authors just think like.  There isn't any known cases of this problem existing since most authors change the name of a class when they absorb an algorithm from another project.

4) For most people how the JavaVM performs its runtime optimization is unknown and a bit of a mystery.  One reason is that the Java's developers are constantly changing the algorithms to improve its performance.  The typical assumption is that the first time some code is run it will be slow then the second time it is run it will be much faster.  Based on experience, the true situation is much more confusion and difficult to understand.

To illustrate how significant this issue is consider the following benchmark that compares performance of three different ways to multiply matrices:
{% highlight java %}
public class BenchmarkMatrixMatrixMult {

    public static long multSmall( DenseMatrix64F matA , DenseMatrix64F matB ,
                             DenseMatrix64F matResult , int numTrials) {
        long prev = System.currentTimeMillis();

        for( int i = 0; i < numTrials; i++ ) {
            MatrixMatrixMult.mult_small(matA,matB,matResult);
        }

        return System.currentTimeMillis()-prev;
    }

    public static void main( String args[] ) {
        int size[] = new int[]{2,4,10,20,50,100,200,500,1000};
        int count[] = new int[]{30000000,6000000,600000,140000,15000,1500,150,8,2,1,1};

        for( int i = 0; i < size.length; i++ ) {
            int width = size[i];
            DenseMatrix64F matA = RandomMatrices.createRandom(width,width,rand);
            DenseMatrix64F matB = RandomMatrices.createRandom(width,width,rand);
            DenseMatrix64F matResult = RandomMatrices.createRandom(width,width,rand);

            long timeSmall = multSmall(matA,matB,matResult,count[i]);
            System.gc();
            long timeAux = multAux(matA,matB,matResult,count[i]);
            System.gc();
            long timeLarge = multLarge(matA,matB,matResult,count[i]);
            System.gc();

            System.out.printf("Size %4d elapsed time: %6d %6d %6d\n",width,timeSmall,timeAux,timeLarge);
        }
    }
}
{% endhighlight %}

Please note that the code has been truncated to only show what is essential.  When the above code is run using JavaVM 1.6.0_15 on my computer it produces the following output:

<pre>
Size    2 elapsed time:   3766   4165   3990
Size    4 elapsed time:   3445   2817   3669
Size   10 elapsed time:   3887   2720   3969
Size   20 elapsed time:   6425   4276   6577
Size   50 elapsed time:   9931   6419  10412
Size  100 elapsed time:   8250   5391   8417
Size  200 elapsed time:   6384   4131   6317
Size  500 elapsed time:   9146   3383   5175
Size 1000 elapsed time:  24195   7191  10677
</pre>

Now let's say that you are only concerned with results for matrices that are 200 or above.  So you simply modify the code so that 'i' is set to 6 initially and you get this output:

<pre>
Size  200 elapsed time:   1798   2316   2198
Size  500 elapsed time:   7556   1941   1855
Size 1000 elapsed time:  13574   3562   3491
</pre>

The results are now completely different!  Why did this happen?  That's hard to say, but clearly the conclusions you reach are dependent upon the order in which you process your results.

### 1.2.1 The Java Matrix Benchmark Approach

Java Matrix Benchmark mitigates these problems by launching a new Java virtual machine with every set of tests for an operation.  In a typical configuration the same operation in the same library will be evaluated 25 times.  A new virtual machine will be launched and it will repeat the test 5 times in that instance.  A new instance is launched 5 times.

Jar files are only loaded if they are used by the library being evaluated.  This eliminates the possibility of accidental overloading of classes.  Between each test in the same Java instance a call to "System.gc()" is made to reduce the influence of the garbage collector on the results.

## <a name="1.3"/>1.3 Benchmark Overhead

When measuring an algorithms performance, the influence of the benchmark on the results should be minimized.  Consider the following benchmark:

{% highlight java %}
int totalTrials = 0;
        
long startTime = System.currentTimeMillis();
while( System.currentTimeMillis() - startTime < 5000 ) {
    for( int i = 0; i < numTrials; i++ ) {
        DenseMatrix64F A = RandomMatrices.createRandom(N,N,rand); 
        operationInterface.perform(A);
    }
}

return (double)(System.currentTimeMillis()-startTime)/(double)totalTrials;
{% endhighlight %}

It counts how many times a specific operation runs in approximately 5 seconds.  Is it really doing that?  What happens if 'createRandom()' takes longer to execute than performOperation()?  Then you are measuring the overhead of the benchmark and not operation itself.  The same is true for 'System.currentTimeMillis()' and for the overhead caused by the use of an interface instead of directly calling the function.

In general large matrices and expensive operations are not significantly affected by this issue.  It can even be interesting to measure the runtime of individual operations for large matrices since more accurate information on the variance can be computed.  However for fast operations or small matrices it is significant. 

### 1.3.1 The Java Matrix Benchmark Approach

A key design requirement of this benchmark was accurate results for small matrices.  All previously know Java benchmarks effectively ignored small matrices and/or had significant overhead in the benchmark.  This was handled by making direct calls to the library's functions between the time samples:
{% highlight java %}
public long process(DenseMatrix64F[]inputs, long numTrials) {
    Matrix matA = convertToJama(inputs[0]);
    Matrix matB = convertToJama(inputs[1]);

    long prev = System.currentTimeMillis();

    for( long i = 0; i < numTrials; i++ ) {
        matA.plus(matB);
    }
    return System.currentTimeMillis()-prev;
}
{% endhighlight %}
The above is the benchmark code is used to evaluate Jama's addition operator.  Note the for loop is the only overhead that is included the elapsed time measurement.

The disadvantage of this approach is that only a few different matrices can be used in testing.  Some algorithm's runtime is dependent on the input they are given.  More matrices that they are evaluated against the better the results will be.  One way around this problem would be to generate a random matrix and then measure the time for a single operation.  For small matrices the execution time is too small to be measured this way and the benchmark will take much longer to run.

## <a name="1.4"/>1.4 Repeatability

Modern operating systems and applications running on them perform many tasks automatically and periodically.  An application could decide that the middle of your benchmark is a good time to download, decompress, and install its latest update.  The OS might decide to update some file indexes.  Some of these can be controlled by turning off services and quitting applications. 

Unfortunately this is a tedious task that often requires intimate knowledge the operating system to effective do.  It is often the case that these tasks will run for a relatively short period of time.  So only one library or set of applications would be influenced.

### 1.4.1 The Java Matrix Benchmark Approach

Java Matrix Benchmark can not eliminate this problem by itself.  It is best for the user to close and kill all non essential applications and processes before running the benchmark.  It does take steps to mitigate the issue by randomizing the order in which it evaluates operations within a library.

By randomizing the order it is less likely that all the individual tests will be slowed down by a short process that suddenly starts up and stops.  A process that takes a lot of time to run will significantly change the results and can be hard to detect.  An additional step that could be taken to improve the situation is to only measure the benchmark application's CPU usage time.  Many operating systems provide this functionality, and ways to get access to that information in an OS independent way inside of Java are being investigated.

## <a name="1.5"/>1.5 Exception Handling

No library is perfect and things will go wrong.  A good benchmark will be able to handle these problems and continue running.  In Java, typical problems include a RuntimeException being thrown, an algorithm not stopping, or threads not terminating.

### 1.5.1 The Java Matrix Benchmark Approach

RuntimeExceptions can be caught with a simple try catch block.  An algorithm caught in an infinite loop is more problematic.  Since each test is a new Java instance it can be easily killed by the master benchmark program if it is taking too long.  All of these exceptions are logged for later debugging of the library or explaining why it went wrong.

# 2 Summary

Here is a summary of how Java Matrix Benchmark evaluates each library:

1. For each operation in each library a new JavaVM instance is launched M times.
2. In each of these instances the operation is tested N times.
  * Results from each of these M*N tests are saved and any exceptions that occurred are logged.
3. Only the Jar files needed by the library being evaluated are loaded in each test's JavaVM instance.
4. How many trials are run in each test is dynamically determined at runtime.
5. Memory allocated to the JavaVM is adjusted for each operation depending on an estimate of how much will be needed.
  * If a library needs additional memory then more is given.
6. The maximum matrix size that is evaluated for each operation in each library is determined at runtime depending on how long it takes to run.
  * An upper limit is specified by the user before hand.
7. The benchmark's overhead's influence on the results is minimized.
8. The order that operations are tested is randomized.

Here are the positive aspects of this approach:

* Accurate runtime performance measurements across a range of matrix sizes.
* One library cannot change the performance of another library that is run later on.  The same is true for operations within a library.
* Short unexpected processes started by the operating system or applications will have a minimal affect on the final results.
* No modifications are needed to run it on different hardware platforms with various capabilities.
* The runtime of the benchmark is approximately constant.

Here are the negatives for this approach:

* Only a small set of matrices are evaluated as input.  Ideally each trial would have its own randomly generated one.
* The variance of individual operations runtime is lost by measuring several of them together.
* CPU time is not currently measured.
* The benchmark code is much more complex.

# Footnotes
(1) This is not true for some real-time implementations of Java.
