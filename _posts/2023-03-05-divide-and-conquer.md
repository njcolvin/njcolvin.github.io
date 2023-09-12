---
layout: distill
title: Recursion
description: an introduction to solving problems recursively
tags: algorithms math
giscus_comments: true
date: 2023-03-05
featured: true

authors:
  - name: Nicholas Colvin
    url: "https://njcolvin.github.io"
    affiliations:
      name: UT Dallas

bibliography: 2023-03-05-divide-and-conquer.bib

# Optionally, you can add a table of contents to your post.
# NOTES:
#   - make sure that TOC names match the actual section names
#     for hyperlinks within the post to work correctly.
#   - we may want to automate TOC generation in the future using
#     jekyll-toc plugin (https://github.com/toshimaru/jekyll-toc).
toc:
  - name: Introduction
  - name: The Substitution Method

---

## Introduction

This note assumes you know what an algorithm is, but as a quick refresher according to Jeff Erickson, "an algorithm is an explicit, precise, unambiguous, mechanically-executable sequence of elementary instructions, usually intended to accomplish a specific purpose."<d-cite key="erickson2019Algos"></d-cite>

Good algorithms are incredibly useful, but sometimes a problem might not have a clear algorithmic solution. Worse, it might even be [impossible](https://en.wikipedia.org/wiki/Undecidable_problem). A powerful method to solve an unknown problem is to make a **reduction** to one or more simpler problems that can be solved by known algorithms. The reduction of a problem can even be into smaller instances of itself, breaking up repeatedly until reaching one of several base cases. This is called **recursion**. Divide-and-conquer is a general pattern for solving a problem using recursion:

1. **Divide** the problem into independent smaller instances of the same problem.
2. **Conquer** or solve the smaller instances by recursion.
3. **Combine** the solutions of the smaller instances into a solution of the original problem.

It is often easier to think of recursion without thinking about solving the smaller subproblems yourself. Instead, think of a worker solving all of the subproblems for you. You must know how to relate the problem to smaller versions of itself, and how to solve any of the smallest instances that can no longer be simplified. Once you simplify the original problem, hand the subproblems off to the worker. They will simplify and solve the subproblems for you, so let them work in peace.

A natural way to characterize the running time of a divide-and-conquer algorithm is to use a **recurrence**, or a function defined in terms of itself. So far we have been using the words _smaller_ and _simpler_ interchangably. Indeed, in terms of analyzing the running time of an algorithm we often consider the input size, denoted $$n$$.

As a quick example, consider the following recurrence for merge sort:

$$
MergeSort(n) = \begin{cases}
  \Theta(1)  & n = 1 \\
  2MergeSort(n/2) + \Theta(n) & otherwise
\end{cases}
$$

Merge sort consists of splitting the unsorted input array into two roughly equal halves, recursively sorting those halves, and then merging the sorted halves into a sorted version of the input array. The recurrence says it takes constant $$\Theta(1)$$ time to sort an array with 1 element, or $$2MergeSort(n/2)$$ time to sort the halves and linear $$\Theta(n)$$ time to merge the halves into a solution. It is well-documented the overall running time is $$\Theta(n \log{n})$$.

***

## The Substitution Method

Consider a similar recurrence:

$$
T(n) = 2T(\lfloor{n/2}\rfloor) + n
$$

This looks like merge sort, so how can we show $$T(n) = O(n \log{n})$$? Using the definition of [big O](https://en.wikipedia.org/wiki/Big_O_notation), we must show there exist positive constants $$c$$ and $$n_0$$ such that:

$$
T(n) \leq cn\lg{n}, \quad n > n_0
$$

To do this, we must assume the above inequality holds for some positive $$m < n$$, particularly $$m = \lfloor{n/2}\rfloor$$. Plugging this in gives:

$$
\begin{flalign}
T(n) & \leq 2(c\lfloor{n/2}\rfloor\lg{\lfloor{n/2}\rfloor}) + n&\\
     & \leq cn\lg{n/2} + n&\\
     & = cn\lg{n} - cn\lg{2} + n&\\
     & = cn\lg{n} - cn + n&\\
     & \leq cn\lg{n} ~ \text{holds for} ~ c \geq 1
\end{flalign}
$$

This looks good for the recurrence, i.e. the inductive step. But we also have to show the inequality holds for the base cases, i.e. for $$c$$ and $$n_0$$. Notice if we choose $$n_0 = 1$$ as the base case, our recurrence says $$T(1) = 2\lfloor{1/2}\rfloor + 1 = 1$$, but the inequality says $$T(1) \leq c1\lg{1} = 0$$, a contradiction!

Luckily, we can remove the troublesome $$n = 1$$ base case from the inductive proof (it is still part of the recurrence, though). Observe that $$T(n)$$ does not depend on $$T(1)$$ for $$n > 3$$. We can replace $$T(1)$$ with $$T(2)$$ and $$T(3)$$ as the base cases in the inductive proof by setting $$n_0 = 2$$. Now we just need to find a value for $$c$$ so that $$T(2) \leq c2\lg{2}$$ and $$T(3) \leq c3\lg{3}$$. Any $$c \geq 2$$ is sufficient.<d-cite key="clrs2009"></d-cite>

This was an example of the **substitution method** for solving recurrences:

1. Guess the form of the solution.
2. Use mathematical induction to find $$c$$ and $$n_0$$ showing the solution works. 