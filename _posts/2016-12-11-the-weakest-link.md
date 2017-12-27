---
layout: post
title: The Weakest Link
---
![The weakest link](https://raw.githubusercontent.com/tdallmann/tdallmann.github.io/master/images/Anne-robinson1.jpg)

You remember the TV game show, where contestants were eliminated by their team because they were the lowest performers - the weakest link? I was reminded of this recently as I pondered [Dr. Eliyahu Goldratt's theory of constraints](https://en.wikipedia.org/wiki/Theory_of_constraints).
## The Theory of Constraints

One of the main tenets of Lean Manufacturing, the theory of constraints basically says "Every process has a constraint (bottleneck) and focusing improvement efforts on that constraint is the fastest and most effective path to improved profitability." (see http://www.leanproduction.com/theory-of-constraints.html) In other words, every complex system consists of multiple linked activities, one of which acts as a constraint upon the **entire system**. The productivity of the entire system is constrained by this single activity.

So lets think about your software delivery value stream: somewhere along the way from idea to production is a bottleneck - the limiting constraint - **_the weakest link_**. This constraint defines the maximum level of productivity you can realize. It also directly correlates to the level of quality you can expect your team to achieve. Efforts you make towards improving any other activity in the process will have **zero impact on the system as a whole**. No matter what you do, until you remove the limiting constraint, your SDLC will flounder.

Here's another way to view it: if you were to make improvements to a process or activity that falls before the limiting constraint, work simply piles up at the bottleneck. I have personally witnessed development teams improve their ability to churn out code quickly, only to find the testing team falling behind on an ever-growing pile of changes to test.

Likewise, if you make improvements to a process or activity that follows the limiting constraint, that activity will find itself "starved" waiting for the bottleneck to generate output. Imagine that your testers have dramatically improved their throughput by automating the majority of test cases. Any time a new story is code-complete, they can churn out new automated tests for it lickity-split. The problem is, the developers can only work so fast, so in between user stories being developed, the testers are left waiting for more work to do. If the testing team was sufficiently keeping up with developers prior to their improvements, the result is a net improvement to the software delivery value stream **of zero**!

## Putting it into Practice

Once you understand this idea, it becomes an exercise in identifying your weakest link, and putting every effort into removing it, ignoring all other constraints until the bottleneck is removed. This focuses you on improving the flow through an entire system, which happens to be the first of ["The Three Ways"](http://itrevolution.com/the-three-ways-principles-underpinning-devops/), the principles underpinning DevOps.

