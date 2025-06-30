---
title: Measuring and Analyzing Algorithm Complexity
description: 'Goals: Looking at different ways of comparing algorithms, Looking at the opinion of bringing out the running period, Looking at the opinion of describing THE WORST, THE BEST and sometimes AVARAGE case running times.'
pubDate: 2013-09-11T17:04:00.000Z
heroImage: '../../assets/Algorithm-1024x295.png'
---

Goals:
*    Looking at different ways of comparing algorithms
*    Looking at the opinion of bringing out the running period
*    Looking at the opinion of describing THE WORST, THE BEST and sometimes AVARAGE case running times.

### To Choose the Algorithm

For each problem there is a crowd of algorithms to solve the problem. Therefore we have an election of algorithms for coding up like programs.

If the program is probable to be used just one time on a little amount of data, then we ought to choose the algorithm which is easiest to carry out. Code it up correctly and run it.

If this program will be used many times and needs much more lifetime. On the other hand, there are also other factors; which is extensibility, reusability, ease of use, portability, readability and performance.

The performance depends on:
- The process it takes the algorithm to carry out
- The network traffic it turns out,
- The space it uses for its variables,
- The amount of disk accesses it does etc.,

Imagine you have 2 or more algorithms that are designed for solving the same problem. There are two main ways of comparing their running process:

**Benchmarking:** Code all them up like programs, carry out both programs on some typical inputs and calculate their running process (running times);

**Analysis:** The reason about the algorithms for developing a formula which are predictive of their process.

 Let’s discuss benchmarking. This one has a few disadvantages:

- If we are choosing between two (or more) algorithms for developing, we’d prefer to guess their running times, compare the guesses and code up only one of them but, benchmarking requires us to code up both algorithms like programs, before we can carry out any comparisons.
- Benchmarking calculates performance on only finite subset of the instances.
