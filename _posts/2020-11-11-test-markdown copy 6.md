---
layout: post
title: Restore 
subtitle: Restore previous system state
tags: [backup, restore, restore previous setting]
comments: true
---

[Power up C++ with the Standard Template Library Part One](https://www.topcoder.com/thrive/articles/Power%20up%20C++%20with%20the%20Standard%20Template%20Library%20Part%20One)

> ### Containers
> 
> Any time you need to operate with many elements you require some kind of container. In native C (not C++) there was only one type of container: the array.  
>   
> The problem is not that arrays are limited (though, for example, it’s impossible to determine the size of array at runtime). Instead, the main problem is that many problems require a container with greater functionality.  
>   
> For example, we may need one or more of the following operations:
> 
> -   Add some string to a container.
>     
> -   Remove a string from a container.
>     
> -   Determine whether a string is present in the container.
>     
> -   Return a number of distinct elements in a container.
>     
> -   Iterate through a container and get a list of added strings in some order.
>     
> 
>   
> Of course, one can implement this functionality in an ordinal array. But the trivial implementation would be very inefficient. You can create the tree- of hash- structure to solve it in a faster way, but think a bit: does the implementation of such a container depend on elements we are going to store? Do we have to re-implement the module to make it functional, for example, for points on a plane but not strings?  
>   
> If not, we can develop the interface for such a container once, and then use everywhere for data of any type. That, in short, is the idea of STL containers.  
>   
>   
>   
> 
> ### Before we begin
> 
> When the program is using STL, it should #include the appropriate standard headers. For most containers the title of standard header matches the name of the container, and no extension is required. For example, if you are going to use stack, just add the following line at the beginning of your program:  
>   
> 
>     1
>     
> 
>   
> Container types (and algorithms, functors and all STL as well) are defined not in global namespace, but in special namespace called “std.” Add the following line after your includes and before the code begin:  
>   
> 
>     1
>     
> 
>   
> Another important thing to remember is that the type of a container is the template parameter. Template parameters are specified with the ‘<’/’>’ “brackets” in code. For example:  
>   
> 
>     1
>     
> 
>   
> When making nested constructions, make sure that the “brackets” are not directly following one another – leave a blank between them.  
>   
> 
>     1
>     2
>     
> 
>   
>   
>   
> 
> ### Vector
> 
> The simplest STL container is vector. Vector is just an array with extended functionality. By the way, vector is the only container that is backward-compatible to native C code – this means that vector actually IS the array, but with some additional features.  
>   
> 
>     1
>     2
>     3
>     4
>     5
>     6
>     7
>     
> 
>   
> Actually, when you type:  
>   
> 
>     1
>     
> 
>   
> The empty vector is created. Be careful with constructions like this:  
>   
> 
>     1
>     
> 
>   
> Here we declare ‘v’ as an array of 10 vector< int >’s, which are initially empty. In most cases, this is not that we want. Use parentheses instead of brackets here. The most frequently used feature of vector is that it can report its size.  
>   
> 
>     1
>     2
>     3
>     4
>     
> 
>   
> Two remarks: first, size() is unsigned, which may sometimes cause problems. Accordingly, I usually define macros, something like sz© that returns size of C as ordinal signed int. Second, it’s not a good practice to compare v.size() to zero if you want to know whether the container is empty. You’re better off using empty() function:  
>   
> 
>     1
>     2
>     
> 
>   
> This is because not all the containers can report their size in O(1), and you definitely should not require counting all elements in a double-linked list just to ensure that it contains at least one.  
>   
> Another very popular function to use in vector is push\_back. Push\_back adds an element to the end of vector, increasing its size by one. Consider the following example:  
>   
> 
>     1
>     2
>     3
>     4
>     5
>     
> 
>   
> Don’t worry about memory allocation — vector will not allocate just one element each time. Instead, vector allocates more memory then it actually needs when adding new elements with push\_back. The only thing you should worry about is memory usage, but at Topcoder this may not matter. (More on vector’s memory policy later.)  
>   
> When you need to resize vector, use the resize() function:  
>   
> 
>     1
>     2
>     3
>     4
>     5
>     6
>     7
>     8
>     
> 
>   
> The resize() function makes vector contain the required number of elements. If you require less elements than vector already contain, the last ones will be deleted. If you ask vector to grow, it will enlarge its size and fill the newly created elements with zeroes.  
>   
> Note that if you use push\_back() after resize(), it will add elements AFTER the newly allocated size, but not INTO it. In the example above the size of the resulting vector is 25, while if we use push\_back() in a second loop, it would be 30.  
>   
> 
>     1
>     2
>     3
>     4
>     5
>     6
>     7
>     8
>     
> 
>   
> To clear a vector use clear() member function. This function makes vector to contain 0 elements. It does not make elements zeroes – watch out – it completely erases the container.  
>   
> There are many ways to initialize vector. You may create vector from another vector:  
>   
> 
>     1
>     2
>     3
>     4
>     
> 
>   
> The initialization of v2 and v3 in the example above are exactly the same.  
>   
> If you want to create a vector of specific size, use the following constructor:  
>   
> 
>     1
>     
> 
>   
> In the example above, the data will contain 1,000 zeroes after creation. Remember to use parentheses, not brackets. If you want vector to be initialized with something else, write it in such manner:  
>   
> 
>     1
>     
> 
>   
> Remember that you can create vectors of any type.  
>   
> Multidimensional arrays are very important. The simplest way to create the two-dimensional array via vector is to create a vector of vectors.  
>   
> 
>     1
>     
> 
>   
> It should be clear to you now how to create the two-dimensional vector of given size:  
>   
> 
>     1
>     2
>     3
>     
> 
>   
> Here we create a matrix of size N\*M and fill it with -1.  
>   
> The simplest way to add data to vector is to use push\_back(). But what if we want to add data somewhere other than the end? There is the insert() member function for this purpose. And there is also the erase() member function to erase elements, as well. But first we need to say a few words about iterators.  
>   
> You should remember one more very important thing: When vector is passed as a parameter to some function, a copy of vector is actually created. It may take a lot of time and memory to create new vectors when they are not really needed. Actually, it’s hard to find a task where the copying of vector is REALLY needed when passing it as a parameter. So, you should never write:  
>   
> 
>     1
>     2
>     3
>     
> 
>   
> Instead, use the following construction:  
>   
> 
>     1
>     2
>     3
>     
> 
>   
> If you are going to change the contents of vector in the function, just omit the ‘const’ modifier.  
>   
> 
>     1
>     2
>     3