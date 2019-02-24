---
permalink: "/runtime/2011_01_ArmCortexA8"
layout: runtime
title:  "Runtime: Arm Cortex-A8"
---

Comments:

* This is an old benchmark done on a gumstix I believe
* trying to make this page work again eyars later

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

| Date      | 2011 / 01 |
|-----------|-----------|
| OS        | ?? |
| Kernel    | |
| CPU       |  |
| RAM       | |
| CPU Cache | |
| JVM       |  |
| Benchmark |  |

| Libraries     | Version    |
|---------------|------------|
| Colt          |         |
| Commons Math  |        |
| EJML          |      |
| Jama          |       |
| JBlas         |       |
| la4j          |       |
| MTJ           |      |
| OjAlgo        |       | 
| Parallel Colt |       |
| UJMP          |       |


# <a name="Summary Results"/>Summary Results 

The following results are a weighted sum across all operations within each matrix size.  Operations which take longer will have more weight.  If a library could not finish an operation then its score is set to zero.  

_An artifact of "could not finish" approach is that libraries which do not support an expensive operation, barely time out, or run out of memory are significantly penalized.  Results around the matrix size of 10k become erratic due to this issue._

**__NOTE__** The weight is computed from the amount of time the fastest library takes to complete.  Which is why the results change a bit from Java to Java + Native.

## <a name="Pure Java Summary Results"/>Pure Java Summary Results

![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/summary.png)
{: .center}

## <a name="Mixed Java and Native Summary Results"/> Mixed Java and Native Summary Results 

![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/native/summary.png)
{: .center}

# <a name="Pure Java Results"/> Pure Java Libraries 

These results show the performance of libraries that have code written entirely in Java.

## <a name="Java: Basic Operation Results"/> Java: Basic Operation Results


| ![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/add.png) | ![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/scale.png) |
| ![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/mult.png) | ![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/multTransB.png) |
| ![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/det.png) | ![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/transpose.png) |
| ![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/invert.png) | ![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/invertSymmPosDef.png) |

## <a name = "Java: Solving Linear Systems"/>Java: Solving Linear Systems

| ![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/solveExact.png) | ![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/solveOver.png) |

## <a name = "Java: Matrix Decompositions"/>Java: Matrix Decompositions

| ![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/svd.png) | ![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/eigSymm.png) |

----

# <a name = "Mixed Java and Native Libraries"/> Mixed Java and Native Libraries

These results show the performance of libraries that either use pure Java or calls to native libraries.

## <a name = "Mixed: Basic Operation Results"/> Mixed: Basic Operation Results

| ![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/native/add.png) | ![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/native/scale.png) |
| ![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/native/mult.png) | ![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/native/multTransB.png) |
| ![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/native/det.png) | ![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/native/transpose.png) |
| ![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/native/invert.png) | ![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/native/invertSymmPosDef.png) |


## <a name = "Mixed: Solving Linear Systems"/> Mixed: Solving Linear Systems

| ![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/native/solveExact.png) | ![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/native/solveOver.png)|

## <a name = "Mixed: Matrix Decompositions"/> Mixed: Matrix Decompositions

| ![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/native/svd.png) | ![]({{site.baseurl}}/runtime/2011_01_ArmCortexA8/native/eigSymm.png) |
