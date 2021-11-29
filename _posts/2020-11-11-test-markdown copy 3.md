---
layout: post
title: Restore 
subtitle: Restore previous system state
tags: [backup, restore, restore previous setting]
comments: true
---


[String Hashing - Competitive Programming Algorithms](https://cp-algorithms.com/string/string-hashing.html)

> # String Hashing
> 
> **Table of Contents**  
> 
> [Calculation of the hash of a string](https://cp-algorithms.com/string/string-hashing.html#toc-tgt-0)
> 
> [Example tasks](https://cp-algorithms.com/string/string-hashing.html#toc-tgt-1)
> 
> [Search for duplicate strings in an array of strings](https://cp-algorithms.com/string/string-hashing.html#toc-tgt-2)
> 
> [Fast hash calculation of substrings of given string](https://cp-algorithms.com/string/string-hashing.html#toc-tgt-3)
> 
> [Applications of Hashing](https://cp-algorithms.com/string/string-hashing.html#toc-tgt-4)
> 
> [Determine the number of different substrings in a string](https://cp-algorithms.com/string/string-hashing.html#toc-tgt-5)
> 
> [Improve no-collision probability](https://cp-algorithms.com/string/string-hashing.html#toc-tgt-6)
> 
> [Practice Problems](https://cp-algorithms.com/string/string-hashing.html#toc-tgt-7)
> 
> Hashing algorithms are helpful in solving a lot of problems.
> 
> We want to solve the problem of comparing strings efficiently. The brute force way of doing so is just to compare the letters of both strings, which has a time complexity of O(min(n1,n2))
> 
> O(\\min(n\_1, n\_2)) if n1n\_1 and n2n\_2 are the sizes of the two strings. We want to do better. The idea behind strings is the following: we convert each string into an integer and compare those instead of the strings. Comparing two strings is then an O(1)
> 
> O(1) operation.
> 
> For the conversion, we need a so-called **hash function**. The goal of it is to convert a string into an integer, the so-called **hash** of the string. The following condition has to hold: if two strings s
> 
> s and tt are equal (s\=ts = t), then also their hashes have to be equal (hash(s)\=hash(t)
> 
> \\text{hash}(s) = \\text{hash}(t)). Otherwise, we will not be able to compare strings.
> 
> Notice, the opposite direction doesn't have to hold. If the hashes are equal (hash(s)\=hash(t)
> 
> \\text{hash}(s) = \\text{hash}(t)), then the strings do not necessarily have to be equal. E.g. a valid hash function would be simply hash(s)\=0\\text{hash}(s) = 0 for each ss. Now, this is just a stupid example, because this function will be completely useless, but it is a valid hash function. The reason why the opposite direction doesn't have to hold, is because there are exponentially many strings. If we only want this hash function to distinguish between all strings consisting of lowercase characters of length smaller than 15, then already the hash wouldn't fit into a 64-bit integer (e.g. unsigned long long) any more, because there are so many of them. And of course, we don't want to compare arbitrary long integers, because this will also have the complexity O(n)
> 
> O(n).
> 
> So usually we want the hash function to map strings onto numbers of a fixed range \[0,m)
> 
> \[0, m), then comparing strings is just a comparison of two integers with a fixed length. And of course, we want hash(s)≠hash(t)\\text{hash}(s) \\neq \\text{hash}(t) to be very likely if s≠t
> 
> s \\neq t.
> 
> That's the important part that you have to keep in mind. Using hashing will not be 100% deterministically correct, because two complete different strings might have the same hash (the hashes collide). However, in a wide majority of tasks, this can be safely ignored as the probability of the hashes of two different strings colliding is still very small. And we will discuss some techniques in this article how to keep the probability of collisions very low.
> 
> ## Calculation of the hash of a string
> 
> The good and widely used way to define the hash of a string s
> 
> s of length n
> 
> n is
> 
> hash(s)\=s\[0\]+s\[1\]⋅p+s\[2\]⋅p2+...+s\[n−1\]⋅pn−1modm\=∑i\=0n−1s\[i\]⋅pimodm,
> 
> \\begin{align} \\text{hash}(s) &= s\[0\] + s\[1\] \\cdot p + s\[2\] \\cdot p^2 + ... + s\[n-1\] \\cdot p^{n-1} \\mod m \\\\ &= \\sum\_{i=0}^{n-1} s\[i\] \\cdot p^i \\mod m, \\end{align}
> 
> where p
> 
> p and m
> 
> m are some chosen, positive numbers. It is called a **polynomial rolling hash function**.
> 
> It is reasonable to make p
> 
> p a prime number roughly equal to the number of characters in the input alphabet. For example, if the input is composed of only lowercase letters of the English alphabet, p\=31p = 31 is a good choice. If the input may contain both uppercase and lowercase letters, then p\=53p = 53 is a possible choice. The code in this article will use p\=31
> 
> p = 31.
> 
> Obviously m
> 
> m should be a large number since the probability of two random strings colliding is about ≈1m\\approx \\frac{1}{m}. Sometimes m\=264m = 2^{64} is chosen, since then the integer overflows of 64-bit integers work exactly like the modulo operation. However, there exists a method, which generates colliding strings (which work independently from the choice of pp). So in practice, m\=264m = 2^{64} is not recommended. A good choice for mm is some large prime number. The code in this article will just use m\=109+9
> 
> m = 10^9+9. This is a large number, but still small enough so that we can perform multiplication of two values using 64-bit integers.
> 
> Here is an example of calculating the hash of a string s
> 
> s, which contains only lowercase letters. We convert each character of ss to an integer. Here we use the conversion a→1a \\rightarrow 1, b→2b \\rightarrow 2, …\\dots, z→26z \\rightarrow 26. Converting a→0a \\rightarrow 0 is not a good idea, because then the hashes of the strings aa, aaaa, aaaaaa, …\\dots all evaluate to 0
> 
> 0.
> 
>     long long compute_hash(string const& s) {
>         const int p = 31;
>         const int m = 1e9 + 9;
>         long long hash_value = 0;
>         long long p_pow = 1;
>         for (char c : s) {
>             hash_value = (hash_value + (c - 'a' + 1) * p_pow) % m;
>             p_pow = (p_pow * p) % m;
>         }
>         return hash_value;
>     }
>     
> 
> Precomputing the powers of p
> 
> p might give a performance boost.
> 
> ## Example tasks
> 
> ### Search for duplicate strings in an array of strings
> 
> Problem: Given a list of n
> 
> n strings sis\_i, each no longer than m
> 
> m characters, find all the duplicate strings and divide them into groups.
> 
> From the obvious algorithm involving sorting the strings, we would get a time complexity of O(nmlogn)
> 
> O(n m \\log n) where the sorting requires O(nlogn)O(n \\log n) comparisons and each comparison take O(m)O(m) time. However, by using hashes, we reduce the comparison time to O(1)O(1), giving us an algorithm that runs in O(nm+nlogn)
> 
> O(n m + n \\log n) time.
> 
> We calculate the hash for each string, sort the hashes together with the indices, and then group the indices by identical hashes.
> 
>     vector<vector<int>> group_identical_strings(vector<string> const& s) {
>         int n = s.size();
>         vector<pair<long long, int>> hashes(n);
>         for (int i = 0; i < n; i++)
>             hashes[i] = {compute_hash(s[i]), i};
>     
>         sort(hashes.begin(), hashes.end());
>     
>         vector<vector<int>> groups;
>         for (int i = 0; i < n; i++) {
>             if (i == 0 || hashes[i].first != hashes[i-1].first)
>                 groups.emplace_back();
>             groups.back().push_back(hashes[i].second);
>         }
>         return groups;
>     }
>     
> 
> ### Fast hash calculation of substrings of given string
> 
> Problem: Given a string s
> 
> s and indices ii and jj, find the hash of the substring s\[i…j\]
> 
> s \[i \\dots j\].
> 
> By definition, we have:
> 
> hash(s\[i…j\])\=∑k\=ijs\[k\]⋅pk−imodm
> 
> \\text{hash}(s\[i \\dots j\]) = \\sum\_{k = i}^j s\[k\] \\cdot p^{k-i} \\mod m
> 
> Multiplying by pi
> 
> p^i gives:
> 
> hash(s\[i…j\])⋅pi\=∑k\=ijs\[k\]⋅pkmodm\=hash(s\[0…j\])−hash(s\[0…i−1\])modm
> 
> \\begin{align} \\text{hash}(s\[i \\dots j\]) \\cdot p^i &= \\sum\_{k = i}^j s\[k\] \\cdot p^k \\mod m \\\\ &= \\text{hash}(s\[0 \\dots j\]) - \\text{hash}(s\[0 \\dots i-1\]) \\mod m \\end{align}
> 
> So by knowing the hash value of each prefix of the string s
> 
> s, we can compute the hash of any substring directly using this formula. The only problem that we face in calculating it is that we must be able to divide hash(s\[0…j\])−hash(s\[0…i−1\])\\text{hash}(s\[0 \\dots j\]) - \\text{hash}(s\[0 \\dots i-1\]) by pip^i. Therefore we need to find the [modular multiplicative inverse](https://cp-algorithms.com/string/algebra/module-inverse.html) of pip^i and then perform multiplication with this inverse. We can precompute the inverse of every pip^i, which allows computing the hash of any substring of ss in O(1)
> 
> O(1) time.
> 
> However, there does exist an easier way. In most cases, rather than calculating the hashes of substring exactly, it is enough to compute the hash multiplied by some power of p
> 
> p. Suppose we have two hashes of two substrings, one multiplied by pip^i and the other by pjp^j. If i<ji < j then we multiply the first hash by pj−ip^{j-i}, otherwise, we multiply the second hash by pi−jp^{i-j}. By doing this, we get both the hashes multiplied by the same power of pp (which is the maximum of ii and j
> 
> j) and now these hashes can be compared easily with no need for any division.
> 
> ## Applications of Hashing
> 
> Here are some typical applications of Hashing:
> 
> -   [Rabin-Karp algorithm](https://cp-algorithms.com/string/string/rabin-karp.html) for pattern matching in a string in O(n)
> 
> -   O(n) time
> -   Calculating the number of different substrings of a string in O(n2logn)
> 
> -   O(n^2 \\log n) (see below)
> -   Calculating the number of palindromic substrings in a string.
> 
> ### Determine the number of different substrings in a string
> 
> Problem: Given a string s
> 
> s of length n
> 
> n, consisting only of lowercase English letters, find the number of different substrings in this string.
> 
> To solve this problem, we iterate over all substring lengths l\=1…n
> 
> l = 1 \\dots n. For every substring length ll we construct an array of hashes of all substrings of length ll multiplied by the same power of pp. The number of different elements in the array is equal to the number of distinct substrings of length l
> 
> l in the string. This number is added to the final answer.
> 
> For convenience, we will use h\[i\]
> 
> h\[i\] as the hash of the prefix with ii characters, and define h\[0\]\=0
> 
> h\[0\] = 0.
> 
>     int count_unique_substrings(string const& s) {
>         int n = s.size();
>     
>         const int p = 31;
>         const int m = 1e9 + 9;
>         vector<long long> p_pow(n);
>         p_pow[0] = 1;
>         for (int i = 1; i < n; i++)
>             p_pow[i] = (p_pow[i-1] * p) % m;
>     
>         vector<long long> h(n + 1, 0);
>         for (int i = 0; i < n; i++)
>             h[i+1] = (h[i] + (s[i] - 'a' + 1) * p_pow[i]) % m;
>     
>         int cnt = 0;
>         for (int l = 1; l <= n; l++) {
>             set<long long> hs;
>             for (int i = 0; i <= n - l; i++) {
>                 long long cur_h = (h[i + l] + m - h[i]) % m;
>                 cur_h = (cur_h * p_pow[n-i-1]) % m;
>                 hs.insert(cur_h);
>             }
>             cnt += hs.size();
>         }
>         return cnt;
>     }
>     
> 
> ## Improve no-collision probability
> 
> Quite often the above mentioned polynomial hash is good enough, and no collisions will happen during tests. Remember, the probability that collision happens is only ≈1m
> 
> \\approx \\frac{1}{m}. For m\=109+9m = 10^9 + 9 the probability is ≈10−9\\approx 10^{-9} which is quite low. But notice, that we only did one comparison. What if we compared a string ss with 10610^6 different strings. The probability that at least one collision happens is now ≈10−3\\approx 10^{-3}. And if we want to compare 10610^6 different strings with each other (e.g. by counting how many unique strings exists), then the probability of at least one collision happening is already ≈1
> 
> \\approx 1. It is pretty much guaranteed that this task will end with a collision and returns the wrong result.
> 
> There is a really easy trick to get better probabilities. We can just compute two different hashes for each string (by using two different p
> 
> p, and/or different mm, and compare these pairs instead. If mm is about 10910^9 for each of the two hash functions than this is more or less equivalent as having one hash function with m≈1018m \\approx 10^{18}. When comparing 10610^6 strings with each other, the probability that at least one collision happens is now reduced to ≈10−6\\approx 10^{-6}.