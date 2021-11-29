---
layout: post
title: Restore 
subtitle: Restore previous system state
tags: [backup, restore, restore previous setting]
comments: true
---

[Power up C++ with the Standard Template Library Part One](https://www.topcoder.com/thrive/articles/Power%20up%20C++%20with%20the%20Standard%20Template%20Library%20Part%20One)

> ### algorithms
> 
> It’s time to speak about algorithms a bit more deeply. Most algorithms are declared in the #include < algorithm > standard header. At first, STL provides three very simple algorithms: min(a,b), max(a,b), swap(a,b). Here min(a,b) and max(a,b) returns the minimum and maximum of two elements, while swap(a,b) swaps two elements.  
>   
> Algorithm sort() is also widely used. The call to sort(begin, end) sorts an interval in ascending order. Notice that sort() requires random access iterators, so it will not work on all containers. However, you probably won’t ever call sort() on set, which is already ordered.  
>   
> You’ve already heard of algorithm find(). The call to find(begin, end, element) returns the iterator where ‘element’ first occurs, or end if the element is not found. Instead of find(…), count(begin, end, element) returns the number of occurrences of an element in a container or a part of a container. Remember that set and map have the member functions find() and count(), which works in O(log N), while std::find() and std::count() take O(N).  
>   
> Other useful algorithms are next\_permutation() and prev\_permutation(). Let’s speak about next\_permutation. The call to next\_permutation(begin, end) makes the interval \[begin, end\] hold the next permutation of the same elements, or returns false if the current permutation is the last one. Accordingly, next\_permutation makes many tasks quite easy. If you want to check all permutations, just write:  
>

``` C++

vector < int > v;

for (int i = 0; i < 10; i++) {
  v.push_back(i);
}

do {
  Solve(…, v);
} while (next_permutation(all(v));
```
>
>
> Don’t forget to ensure that the elements in a container are sorted before your first call to next\_permutation(…). Their initial state should form the very first permutation; otherwise, some permutations will not be checked.