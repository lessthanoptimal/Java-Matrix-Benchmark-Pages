---
permalink: "/runtime/2018_04_XeonQuad/"
layout: runtime
title:  "Runtime: Cloud - Xeon 4 thread"
---

Comments:

* Benchmark has been re-written to use Gradle and download jars from Maven Central
* Benchmark run by [Anders Peterson](https://ojalgo.blogspot.com/2018/05/new-java-matrix-benchmark-results-coming.html)
* Most complete benchmark yet! All libraries up to size 10000 and took two months to run.

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

| Date      | 2018 / 04 |
|-----------|-----------|
| Instance  | Google Cloud n1-highmem-4 |
| OS        | CentOS 7.5.1804 |
| CPU       | Skylake Xeon |
| RAM       | 26 GB |
| CPU Cache | Unknown |
| JVM       | Oracle Java HotSpot(TM) 64-Bit Server 1.8.0_161 |
| Benchmark | 0.11 |

| Libraries     | Version    |
|---------------|------------|
| Colt          |  1.2       |
| Commons Math  | 3.6.1      |
| EJML          |  0.35      |
| Jama          | 1.0.3      |
| JBlas         | 1.2.4      |
| la4j          | 0.6.0      |
| MTJ           | 1.0.7      |
| OjAlgo        | 45.0.0     | 
| UJMP          | 0.3.0      |

# <a name="Summary Results"/>Summary Results 

The following results are a weighted sum across all operations within each matrix size.  Operations which take longer will have more weight.  If a library could not finish an operation then its score is set to zero.  

**NOT** The weight is computed from the amount of time the fastest library takes to complete.  Which is why the results change a bit from Java to Java + Native.

## <a name="Pure Java Summary Results"/>Pure Java Summary Results

![]({{site.baseurl}}/runtime/2018_04_XeonQuad/summary.png)
{: .center}

## <a name="Mixed Java and Native Summary Results"/> Mixed Java and Native Summary Results 

![]({{site.baseurl}}/runtime/2018_04_XeonQuad/native/summary.png)
{: .center}

# <a name="Pure Java Results"/> Pure Java Libraries 

These results show the performance of libraries that have code written entirely in Java.

## <a name="Java: Basic Operation Results"/> Java: Basic Operation Results


| ![]({{site.baseurl}}/runtime/2018_04_XeonQuad/add.png) | ![]({{site.baseurl}}/runtime/2018_04_XeonQuad/scale.png) |
| ![]({{site.baseurl}}/runtime/2018_04_XeonQuad/mult.png) | ![]({{site.baseurl}}/runtime/2018_04_XeonQuad/multTransB.png) |
| ![]({{site.baseurl}}/runtime/2018_04_XeonQuad/det.png) | ![]({{site.baseurl}}/runtime/2018_04_XeonQuad/transpose.png) |
| ![]({{site.baseurl}}/runtime/2018_04_XeonQuad/invert.png) | ![]({{site.baseurl}}/runtime/2018_04_XeonQuad/invertSymmPosDef.png) |

## <a name = "Java: Solving Linear Systems"/>Java: Solving Linear Systems

| ![]({{site.baseurl}}/runtime/2018_04_XeonQuad/solveExact.png) | ![]({{site.baseurl}}/runtime/2018_04_XeonQuad/solveOver.png) |

## <a name = "Java: Matrix Decompositions"/>Java: Matrix Decompositions

| ![]({{site.baseurl}}/runtime/2018_04_XeonQuad/svd.png) | ![]({{site.baseurl}}/runtime/2018_04_XeonQuad/eigSymm.png) |

----

# <a name = "Mixed Java and Native Libraries"/> Mixed Java and Native Libraries

These results show the performance of libraries that either use pure Java or calls to native libraries.

## <a name = "Mixed: Basic Operation Results"/> Mixed: Basic Operation Results

| ![]({{site.baseurl}}/runtime/2018_04_XeonQuad/native/add.png) | ![]({{site.baseurl}}/runtime/2018_04_XeonQuad/native/scale.png) |
| ![]({{site.baseurl}}/runtime/2018_04_XeonQuad/native/mult.png) | ![]({{site.baseurl}}/runtime/2018_04_XeonQuad/native/multTransB.png) |
| ![]({{site.baseurl}}/runtime/2018_04_XeonQuad/native/det.png) | ![]({{site.baseurl}}/runtime/2018_04_XeonQuad/native/transpose.png) |
| ![]({{site.baseurl}}/runtime/2018_04_XeonQuad/native/invert.png) | ![]({{site.baseurl}}/runtime/2018_04_XeonQuad/native/invertSymmPosDef.png) |


## <a name = "Mixed: Solving Linear Systems"/> Mixed: Solving Linear Systems

| ![]({{site.baseurl}}/runtime/2018_04_XeonQuad/native/solveExact.png) | ![]({{site.baseurl}}/runtime/2018_04_XeonQuad/native/solveOver.png)|

## <a name = "Mixed: Matrix Decompositions"/> Mixed: Matrix Decompositions

| ![]({{site.baseurl}}/runtime/2018_04_XeonQuad/native/svd.png) | ![]({{site.baseurl}}/runtime/2018_04_XeonQuad/native/eigSymm.png) |
