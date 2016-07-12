+++
date = "2016-07-05T08:36:54-07:00"
title = "Analysing random algorithms (Part I): deriving quicksort's time complexity"
+++


Analysis of randomized algorithms (Part I)
==============================
## Estimating the quicksort's time performance


<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>



### *Abstract:* Building intuition for randomized algorithms and deriving the processing time asymptotic behaviour for quicksort


Randomized algorithms are fascinating and somewhat misterious: they can deliver surprising efficiency but being able to actually understand what gives them their properties can be pretty hard at times.



Specifically, deriving the execution time estimation can get pretty challenging in general case, in a sharp contrast to the deterministic algorithms. For the average execution time, for example, we have to consider each of the possible random outcomes along with its probability and then average out over all of them. Not to mention that just the average time may not enough to characterize the alorithm and we may have to investigate metrics beyond just the average. The mathematical expressions behind those easily gets so hairy that they tend to be omitted at all in many educational materials and textbooks.

However, that complexity is also a bit deceptive in practice. The key is that we are usually only interested in asymptotics, that is the behaviour in some limit, such as the limit of large dataset. That simplifies the problem immensely and means that, with a bit of experience behind our belt, we can learn to arrive at the answer pretty easily. It all comes down to carefully studying what terms are important in our case of interest and what are not.

Let's illustrate this with an example. We will look at the asymptotics of what is probably the most famous random-based algorithm: the quicksort.


## Heads on

We will be interested in the asymptotic limit of large arrays: that is, when the number of elements `n` in the array we need to sort gets pretty big. Let's denote this execution time for quicksort with `f(n)`.

The challenge here is that we don't know how lucky we are going to get with our random choice of the pivot element: it could be the median which means we divided our problem into two smaller ones perfectly, or it could be the smallest element and we may have shifted the whole array to only "sort" one element.

Let's assume all the elements are different for simplicity and the pivot happened to be the `m` the element of the array. We know the execution time for the given value `m`: at each iteration, we would have to go through the entire array, then sort the both subsets with times `f(m)` and `f(n-m)` for each:

<p style="text-align:center">
  ` f(m) + f(n-m) + c_{1} + c_{2} n`
</p>

The pivot element choice being random means that the execution time is the average over all possible values `m`, from 1 up to `n`:

<p style="text-align:center">
  `f(n) = \frac{1}{n} \sum_{m=1...n} [ f(m) + f(n-m) + c_{1} + c_{2} n ] `
</p>

This expression does not look very friendly. Let's start with building some intuition for what the solution might look like.


## Boundaries

Let's see what kind of solution we may expect here.   

Why do we need to go through the derivation at all? Unless you are working in alorithms research, it is unlikely that we would ever . Numerical simulation is good enough. My reason is that it helps build a proper understanding of what is going on: see which decisions in our design matter and which are not.

That is why the best thing is to look into the custom cases what kind of answer we may reasonably expect.

Let's look at the worst case `O(n)`: this case unlikiest ever: every time we manage to make the worst pivot element ever: the smaller subset only has the pivot element, and the other one contains all the other ones, `n-1`. We then we have to make the recursive call on that set.   

<p style="text-align:center">
  `f_{\text{worst}}(n) = f_{\text{worst}}(n-1) + n`
</p>

We can just unfold the expression recursively:

<p style="text-align:center">
  `f_{\text{worst}}(n) = f_{\text{worst}}(n-1) + n = f_{\text{worst}}(n-2) + (n-1) + n = f_{\text{worst}}(1) + \sum_{x=1..n} x`
</p>


Since we are only interested in asymptotics we don't even need to be careful with the arithmetic sum expression  `1 + 2 + 3 + .. + n`. We can just apply the fact that with the paring up of the elements, the average is about `x/2` and we get `x` of them, so

<p style="text-align:center">
  `f_{\text{worst}}(n) = n^2`
</p>

In the worst case, quicksort looks like the naive sort implementation: we keep selecting the smallest (or the biggest) element of the array, insert it into the very beginning (or end).

Now let's look at the best case: when every pivot we choose turns out to be the median element of the array! The two subsets are then exactly half in size:

<p style="text-align:center">
  `f_{\text{best}}(n) = 2 \cdot f_{\text{best}}(\frac{n}{2}) + n`
</p>


Continuing iteration, we get

<p style="text-align:center">
  `f_{\text{best}}(n) = 2 \cdot f_{\text{best}}(\frac{n}{2}) + n = 4 \cdot f_{\text{best}}(\frac{n}{4}) + 3*n`
</p>


This recursive expression looks similar to the one we for the worst case, except. Let's make it the iterative with rewriting this relation in terms of a new variable `x ~ log n`:  

<p style="text-align:center">
 `n=2^{x} \text{ or } x= \{log}_{2} n`



Now we can express the same relation in terms of `g(x)=f(n(x))`:

<p style="text-align:center">
  `g(x) = 2 g(x-1) + 2^{x}`
</p>

Solving this relation gets us

<p style="text-align:center">
  `g(x-1) = 2 g(x-2) + \frac{1}{2} 2^{x}`
</p>

<p style="text-align:center">
  `g(x) = 2 g(x-1) + 2^{x} = 4 g(x-2) + 2 \cdot \frac{1}{2} 2^{x} + 2^{x} = .. = g(1) + x \cdot 2^{x}`
</p>

so the solution is `f(n) ~ n log (n)`.
