---
layout: post
title: Restore 
subtitle: Restore previous system state
tags: [backup, restore, restore previous setting]
comments: true
---

[Power up C++ with the Standard Template Library Part One](https://www.topcoder.com/thrive/articles/Power%20up%20C++%20with%20the%20Standard%20Template%20Library%20Part%20One)

> ### String
> 
> There is a special container to manipulate with strings. The string container has a few differences from vector< char >. Most of the differences come down to string manipulation functions and memory management policy.  
>   
> String has a substring function without iterators, just indices:  
>   
> 
>     1
>     2
>     3
>     4
>     5
>     6
>     
> 
>   
> Beware of (s.length()-1) on empty string because s.length() is unsigned and unsigned(0) – 1 is definitely not what you are expecting!  
>   
>   
>   
> 
> ### Set
> 
> It’s always hard to decide which kind of container to describe first – set or map. My opinion is that, if the reader has a basic knowledge of algorithms, beginning from ‘set’ should be easier to understand.  
>   
> Consider we need a container with the following features:
> 
> -   add an element, but do not allow duples \[duplicates?\]
>     
> -   remove elements
>     
> -   get count of elements (distinct elements)
>     
> -   check whether elements are present in set
>     
> 
>   
> This is quite a frequently used task. STL provides the special container for it – set. Set can add, remove and check the presence of particular element in O(log N), where N is the count of objects in the set. While adding elements to set, the duples \[duplicates?\] are discarded. A count of the elements in the set, N, is returned in O(1). We will speak of the algorithmic implementation of set and map later — for now, let’s investigate its interface:  
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
>     9
>     10
>     11
>     12
>     
> 
>   
> The push\_back() member may not be used with set. It make sense: since the order of elements in set does not matter, push\_back() is not applicable here.  
>   
> Since set is not a linear container, it’s impossible to take the element in set by index. Therefore, the only way to traverse the elements of set is to use iterators.  
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
> It’s more elegant to use traversing macros here. Why? Imagine you have a set< pair<string, pair< int, vector< int > > >. How to traverse it? Write down the iterator type name? Oh, no. Use our traverse macros instead.  
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
> Notice the ‘it->second.first’ syntax. Since ‘it’ is an iterator, we need to take an object from ‘it’ before operating. So, the correct syntax would be ‘(\*it).second.first’. However, it’s easier to write ‘something->’ than ‘(\*something)’. The full explanation will be quite long –just remember that, for iterators, both syntaxes are allowed.  
>   
> To determine whether some element is present in set use ‘find()’ member function. Don’t be confused, though: there are several ‘find()’ ’s in STL. There is a global algorithm ‘find()’, which takes two iterators, element, and works for O(N). It is possible to use it for searching for element in set, but why use an O(N) algorithm while there exists an O(log N) one? While searching in set and map (and also in multiset/multimap, hash\_map/hash\_set, etc.) do not use global find – instead, use member function ‘set::find()’. As ‘ordinal’ find, set::find will return an iterator, either to the element found, or to ‘end()’. So, the element presence check looks like this:  
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
> Another algorithm that works for O(log N) while called as member function is count. Some people think that:  
>   
> 
>     1
>     2
>     3
>     
> 
>   
> Or even  
>   
> 
>     1
>     2
>     3
>     
> 
>   
> is easier to write. Personally, I don’t think so. Using count() in set/map is nonsense: the element either presents or not. As for me, I prefer to use the following two macros:  
>   
> 
>     1
>     2
>     
> 
>   
> (Remember that all© stands for “c.begin(), c.end()”)  
>   
> Here, ‘present()’ returns whether the element presents in the container with member function ‘find()’ (i.e. set/map, etc.) while ‘cpresent’ is for vector.  
>   
> To erase an element from set use the erase() function.  
>   
> 
>     1
>     2
>     3
>     4
>     
> 
>   
> The erase() function also has the interval form:  
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
> Set has an interval constructor:  
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
> It gives us a simple way to get rid of duplicates in vector, and sort it:  
>   
> 
>     1
>     2
>     3
>     4
>     
> 
>   
> Here ‘v2′ will contain the same elements as ‘v’ but sorted in ascending order and with duplicates removed.  
>   
> Any comparable elements can be stored in set. This will be described later.  
>   
>   
>   
> 
> ### Map
> 
> There are two explanation of map. The simple explanation is the following:  
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
>     9
>     10
>     
> 
>   
> Very simple, isn’t it?  
>   
> Actually map is very much like set, except it contains not just values but pairs <key, value>. Map ensures that at most one pair with specific key exists. Another quite pleasant thing is that map has operator \[\] defined.  
>   
> Traversing map is easy with our ‘tr()’ macros. Notice that iterator will be an std::pair of key and value. So, to get the value use it->second. The example follows:  
>   
> 
>     1
>     2
>     3
>     4
>     5
>     6
>     
> 
>   
> Don’t change the key of map element by iterator, because it may break the integrity of map internal data structure (see below).  
>   
> There is one important difference between map::find() and map::operator \[\]. While map::find() will never change the contents of map, operator \[\] will create an element if it does not exist. In some cases this could be very convenient, but it’s definitly a bad idea to use operator \[\] many times in a loop, when you do not want to add new elements. That’s why operator \[\] may not be used if map is passed as a const reference parameter to some function:  
>   
> 
>     1
>     2
>     3
>     4
>     5
>     6
>     7