---
permalink: "/manual/DescriptionStability/"
layout: stability
title:  "Description of Numerical Stability Benchmark"
---

Stability and accuracy are two very important issues in computational linear algebra.  Stability deals with how sensitive a system is to small perbations or how well it performs under certain stressing conditions.  Accuracy provides a metric how different the computed answer is from the true answer.  

To evaluate the quality of different linear algebra libraries, tests are performed on several important decomposition algorithms and linear solvers in each library.  Three types of tests are performed for all algorithms; accuracy, overflow, and underflow.  A fourth type of test is also done for linear solvers that test their ability to handle nearly singular systems.  Accuracy tests the ability to accurately decompose or solve a well formed system.  Overflow and underflow test their ability to handle very large and small numbers respectively.  

This benchmark does not claim to test for every possible type of error.  It is entirely possible for a library to pass all of these tests and still produce poor results in a different application.  The tests were selected to provide a general feel for a libraries accuracy and expose some common problems.

# Testing Procedure

Each test is performed several times using randomly generated matrices.  The tests are also performed across a range of matrix sizes.  Randomly generated matrices are used to provide a good range of tests.  The disadvantage of this approach is that particularly stressing cases might be missed.    In general the specialized stressing cases are specific for a particular algorithm/implementation and are not included since this is a general purpose benchmark.  It is possible for an algorithm to perform very well in these tests, but perform poorly for a particular application.

There are two reason for testing different sizes of matrices.  Some libraries switch between different algorithms depending on the size and some errors become more noticeable for larger matrices.  The size of each matrix is specified on the results page.

## Metrics

Depending on the test different metrics are used.  Accuracy is measured as the frobenius norm of the difference between the expected and found results.  The specifics for how this is computed for each solver/decomposition is provided below.  Overflow/underflow tests work by scaling up or down a matrix until a stopping condition is encountered.  For overflow tests larger numbers are better with infinity being the best.  For underflow small numbers are better.

### Trial Stopping Condition

There are special different reasons for an individual test to stop, which are shown below:

|----------|-------------------------|
| Finished | The test finished with no error. |
| Large Error | The difference from the expected result was too large. |
| Uncountable | A matrix was returned with at least one element that was not a countable number (infinity or NaN) |
| Exception | An unexpected exception was thrown.  For example a null pointer exception would be unexpected |
| Detected | The algorithm detected that it has failed. For example, a SingularMatrix exception could be thrown or value of false could be returned.  |

On the results page the fraction of times that each of these stopping conditions was encountered is listed.

### Fatal Test Errors

A fatal error for a test is an error that prevents results from being generated.  There are two types of fatal errors; froze and memory.  A test is declared frozen is some significant amount of time passes and no results are generated.  Memory indicates that not enough memory was allocated for the test to run.  This can be caused by the library requiring much more than the expected amount or the system not having enough memory.

It is possible for a frozen test to not really be frozen, it could just be running very slowly.  Unfortunately it is impossible to tell the difference between the two.  As a general rule for small and medium matrix tests if it times out then it is a bug in the algorithm.  For large matrices it might just be slow but it could also be frozen.


### Understanding Overflow/Underflow and Nearly Singular Results

<TABLE>
<TR><TH></TH><TH colspan="8"> Overflow </TH></TR>
<TR><TH/><TH>Fatal</TH><TH>Metric 10%</TH><TH>Metric 50%</TH><TH>Metric 90%</TH><TH>Uncountable</TH><TH>Exception</TH><TH>Large Error</TH><TH>Detected</TH></TR>
<TR><TH>Lib A</TH><TD>TIME</TD><TD></TD><TD></TD><TD></TD><TD></TD><TD></TD><TD></TD><TD></TD></TR>
<TR><TH>Lib B</TH><TD/><TD>Infinity</TD><TD>Infinity</TD><TD>Infinity</TD><TD></TD><TD></TD><TD></TD><TD></TD></TR>
<TR><TH>Lib C</TH><TD/><TD> 1.0e+85</TD><TD> 1.0e+85</TD><TD>1.0e+155</TD><TD> 37.2%</TD><TD> 62.8%</TD><TD></TD><TD></TD></TR>
<TR><TH>Lib D</TH><TD/><TD> 1.0e+00</TD><TD> 1.0e+00</TD><TD> 1.0e+00</TD><TD> 52.7%</TD><TD></TD><TD> 30.8%</TD><TD> 16.5%</TD></TR>
<TR><TH>Lib E</TH><TD/><TD/><TD/><TD/><TD/><TD/><TD/><TD/></TR>
</TABLE>


The above table is an example of what results from the overflow/underflow benchmark might look like.  

* First column indicates the library.
* "Fatal" column indicates if a fatal error occurred and what it was if one did. If a fatal error happens there are no results.
* Next three columns are the percentile statics for the computed metric.  For example 10% indicates that 10% of the trials had a metric score less than the specified value.
* "Uncountable" column shows the percent of time a trial returned a matrix that was uncountable.
* "Exception" column shows the percent of time a trial returned threw an unexpected exception..
* "Large Error" column shows the percent of time a trial returned a matrix that had an excessively large error.
* "Detected" column shows the percent of time a trial the algorithm detected an error and stopped gracefully.

The metric being shown is the size of the scaling factor or size of the smallest singular value at the time the test stopped, for overflow/underflow and nearly singular tests respectively. A value of infinity for overflow or zero for underflow indicates that a terminating condition was never encountered before the scaling factor exceeded its numerical limits.  This is the best that an algorithm can possibly perform.  For sake of brevity the percent of the time in which an algorithm finished without error is not shown.  As a result the percentages do not always add up to one.

Notice how "Lib E" has no results.  This is because the library did not support the particular operation being evaluated.

### Understanding Accuracy Results

<TABLE>
<TR><TH></TH><TH colspan="8"> Accuracy </TH></TR>
<TR><TH/><TH>Fatal</TH><TH>Metric 10%</TH><TH>Metric 50%</TH><TH>Metric 90%</TH><TH>Uncountable</TH><TH>Exception</TH><TH>Large Error</TH><TH>Detected</TH></TR>
<TR><TH>Lib A</TH><TD/><TD> 5.1e-16</TD><TD> 1.2e-15</TD><TD> 1.6e-15</TD><TD></TD><TD>5%</TD><TD></TD><TD></TD></TR>
<TR><TH>Lib B</TH><TD/><TD/><TD/><TD/><TD/><TD/><TD/><TD/></TR>
</TABLE>

The above table is an example of what the results from an accuracy benchmark might look like.  This table is similar to the overflow/underflow charts.  The metric being evaluated is accuracy.  If a trial failed then a value of NaN was recorded as the result.

One major difference is the meaning of error columns.  Instead of indicating a terminating condition they show how often a specific type of error occurred.  For example, if an unexpected exception was thrown, then it produced no result and NaN was recorded as the metric.

## Raw Results 

The raw results contains more information than is presented on the website.  Of interest to library developers is the list of exceptions that were thrown.  A link is provided

# Benchmarks 

## Solve: Linear and Least Squares 

Both linear and least squares problems are described by the following equation:
Ax = y
where A is m by n, x is n by 1 and y is m by 1.  For linear equations m = n and for least squares m > n.  The matrix x is unknown and solved for.

Accuracy is computed with:

error = |y-Ax|

All tests start with a randomly generated matrix whose singular values are all one.  Such matrices can be easily solved for.  For the accuracy test this matrix is used as is, for overflow/underflow tests it is scaled up/down.  In the nearly singular test one singular value is randomly selected and scaled down.  The smaller the singular value is before the algorithm breaks the better.

## Singular Value Decomposition

The accuracy of SVD is computed with:

error = |A-USV<sup>T</sup>|

where U,S, and V is the SVD of A.  It is tested against randomly generated matrices that have a singular value of one.

To ensure reasonable scaling the test matrices have all have singular values of one initially.
*NOTE: Change this to nearly one to avoid an unrealistic pathological case*

## Eigenvalue Decomposition

The accuracy of an Eigenvalue decomposition is computed with:

error = |AV - LV|

where V is a matrix composed of the eigenvectors and L is a matrix composed of the eigenvalues.  If the eigenvalues are all positive then it is a diagonal matrix.

Currently only symmetric matrices are tested.  This problem is important in many engineering and scientific applications and has a solution contained in the real domain.  The general eigenvalue problem involves imaginary numbers, which goes beyond the scale of this benchmark.

To ensure reasonable scaling the test matrices have all have singular values of one initially.
*NOTE: Change this to nearly one to avoid an unrealistic pathological case*

# Discussion

How practical and realistic are these tests?  That depends on the test.  The overflow/underflow tests take each library well beyond the typical range.  Accuracy tests provide an idea of how accurate an algorithm is when processing very well formed matrices.  For the linear and least-squares solvers nearly singular systems are not uncommon, but in many cases one would use SVD instead if it was likely that a singular or nearly singular system was being processed.

The overflow and underflow tests are extreme cases and it is unlikely that any real world engineering problem would produce numbers that large or small.  In fact prescaling of each matrix before processing would mitigate this issue.  However, the test do expose potential problems in an algorithm and indicate how careful a user needs to be when using the library. 
