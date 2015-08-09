---
permalink: "/runtime/2015_07_XeonQuad/"
layout: runtime
title:  "Runtime: 2.26 Ghz Intel Xeon"
---

Comments:

* Parallel colt was ommitted due to some difficulties in running it
* Benchmark run by Anders Peterson

Links to the results:

* [Summary Results](#Summary Results)
* [Pure Java Results](#Pure Java Results)
  * [Basic Operations](#Java: Basic Operation Results)
  * [Solving Linear Systems](#Java: Solving Linear Systems)
  * [Matrix Decompositions](#Java: Matrix Decompositions)
* [Java and Native Results](#Mixed Java and Native Libraries)
  * [Basic Operations](#Mixed: Basic Operation Results)
  * [Solving Linear Systems](#Mixed: Solving Linear Systems)
  * [Matrix Decompositions](#Mixed: Matrix Decompositions)

# Test Environment

UPDATE

| Date      | 2015 / 07 |
|-----------|-----------|
| OS        | Mac OS X 10.10.4 64-bit|
| CPU       | 2x2.26 Quad-Core Intel Xeon. 16-threads |
| RAM       | 12 G |
| CPU Cache | 8 MB L3, 256kB L2, 32kB L1 |
| JVM       | Oracle Java HotSpot(TM) 64-Bit Server 1.8.0_45 |
| Benchmark | 0.10 |

| Libraries     | Version    |
|---------------|------------|
| Colt          |  1.2       |
| Commons Math  | 3.5        |
| EJML          |  0.27      |
| Jama          | 1.0.3      |
| JBlas         | 1.2.4      |
| la4j          | 0.5.5      |
| MTJ           | 1.0.3      |
| OjAlgo        | 38.1       | 
| UJMP          | 0.2.5      |

# <a name="Summary Results"/>Summary Results 

The following results are a weighted sum across all operations within each matrix size.  Operations which take longer will have more weight.  If a library could not finish an operation then its score is set to zero.  

**__NOTE__** The weight is computed from the amount of time the fastest library takes to complete.  Which is why the results change a bit from Java to Java + Native.

## <a name="Pure Java Summary Results"/>Pure Java Summary Results

![]({{site.baseurl}}/runtime/2015_07_XeonQuad/summary.png)
{: .center}

## <a name="Mixed Java and Native Summary Results"/> Mixed Java and Native Summary Results 

![]({{site.baseurl}}/runtime/2015_07_XeonQuad/native/summary.png)
{: .center}

# <a name="Pure Java Results"/> Pure Java Libraries 

These results show the performance of libraries that have code written entirely in Java.

## <a name="Java: Basic Operation Results"/> Java: Basic Operation Results


| ![]({{site.baseurl}}/runtime/2015_07_XeonQuad/add.png) | ![]({{site.baseurl}}/runtime/2015_07_XeonQuad/scale.png) |
| ![]({{site.baseurl}}/runtime/2015_07_XeonQuad/mult.png) | ![]({{site.baseurl}}/runtime/2015_07_XeonQuad/multTransB.png) |
| ![]({{site.baseurl}}/runtime/2015_07_XeonQuad/det.png) | ![]({{site.baseurl}}/runtime/2015_07_XeonQuad/transpose.png) |
| ![]({{site.baseurl}}/runtime/2015_07_XeonQuad/invert.png) | ![]({{site.baseurl}}/runtime/2015_07_XeonQuad/invertSymmPosDef.png) |

## <a name = "Java: Solving Linear Systems"/>Java: Solving Linear Systems

| ![]({{site.baseurl}}/runtime/2015_07_XeonQuad/solveExact.png) | ![]({{site.baseurl}}/runtime/2015_07_XeonQuad/solveOver.png) |

## <a name = "Java: Matrix Decompositions"/>Java: Matrix Decompositions

| ![]({{site.baseurl}}/runtime/2015_07_XeonQuad/svd.png) | ![]({{site.baseurl}}/runtime/2015_07_XeonQuad/eigSymm.png) |

----

# <a name = "Mixed Java and Native Libraries"/> Mixed Java and Native Libraries

These results show the performance of libraries that either use pure Java or calls to native libraries.

## <a name = "Mixed: Basic Operation Results"/> Mixed: Basic Operation Results

| ![]({{site.baseurl}}/runtime/2015_07_XeonQuad/native/add.png) | ![]({{site.baseurl}}/runtime/2015_07_XeonQuad/native/scale.png) |
| ![]({{site.baseurl}}/runtime/2015_07_XeonQuad/native/mult.png) | ![]({{site.baseurl}}/runtime/2015_07_XeonQuad/native/multTransB.png) |
| ![]({{site.baseurl}}/runtime/2015_07_XeonQuad/native/det.png) | ![]({{site.baseurl}}/runtime/2015_07_XeonQuad/native/transpose.png) |
| ![]({{site.baseurl}}/runtime/2015_07_XeonQuad/native/invert.png) | ![]({{site.baseurl}}/runtime/2015_07_XeonQuad/native/invertSymmPosDef.png) |


## <a name = "Mixed: Solving Linear Systems"/> Mixed: Solving Linear Systems

| ![]({{site.baseurl}}/runtime/2015_07_XeonQuad/native/solveExact.png) | ![]({{site.baseurl}}/runtime/2015_07_XeonQuad/native/solveOver.png)|

## <a name = "Mixed: Matrix Decompositions"/> Mixed: Matrix Decompositions

| ![]({{site.baseurl}}/runtime/2015_07_XeonQuad/native/svd.png) | ![]({{site.baseurl}}/runtime/2015_07_XeonQuad/native/eigSymm.png) |
