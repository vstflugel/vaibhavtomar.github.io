---
layout: post
title: Divide and conquer 
subtitle: Strategy
tags: [Divide and Conquer, Binary search]
comments: true
---


## Divide and Conquer algorithm

> The divide-and-conquer strategy solves a problem by:
>
> 1. Breaking it into subproblems that are themselves smaller instances of the same type of problem
> 2. Recursively solving these subproblems
> 3. Appropriately combining their answers
>
> `Binary search` :- The ultimate divide-and-conquer algorithm is, of course, binary search: to find a key k in a large file containing keys z[0, 1, . . . , n − 1] in sorted order, we first compare k with z[n/2], and depending on the result we recurse either on the first half of the file, z[0, . . . , n/2 − 1], or on the second half, z[n/2, . . . , n − 1]. The recurrence now is T (n) = T (dn/2e) + O(1), which is the case a = 1, b = 2, d = 0. Plugging into our master theorem we get the familiar solution: a running time of just O(log n).
>
> * Problem statement:- product of two d degree polynomial.
>
>   * First fourier transformation algorithm for dlog(d) complexity. This will require the use of complex number to compute. Such that the evalutaion matrix exactly matches the fourier tramsformation. So we will to nearest exact power of 2. One of the most popular algorithm in the world