---
layout: post
title: Restore 
subtitle: Restore previous system state
tags: [backup, restore, restore previous setting]
comments: true
---

[Prefix function. Knuth–Morris–Pratt algorithm - Competitive Programming Algorithms](https://cp-algorithms.com/string/prefix-function.html)

> # Prefix function. Knuth–Morris–Pratt algorithm
> 
> **Table of Contents**  
> 
> [Prefix function definition](https://cp-algorithms.com/string/prefix-function.html#toc-tgt-0)
> 
> [Trivial Algorithm](https://cp-algorithms.com/string/prefix-function.html#toc-tgt-1)
> 
> [Efficient Algorithm](https://cp-algorithms.com/string/prefix-function.html#toc-tgt-2)
> 
> [First optimization](https://cp-algorithms.com/string/prefix-function.html#toc-tgt-3)
> 
> [Second optimization](https://cp-algorithms.com/string/prefix-function.html#toc-tgt-4)
> 
> [Final algorithm](https://cp-algorithms.com/string/prefix-function.html#toc-tgt-5)
> 
> [Implementation](https://cp-algorithms.com/string/prefix-function.html#toc-tgt-6)
> 
> [Applications](https://cp-algorithms.com/string/prefix-function.html#toc-tgt-7)
> 
> [Search for a substring in a string. The Knuth-Morris-Pratt algorithm](https://cp-algorithms.com/string/prefix-function.html#toc-tgt-8)
> 
> [Counting the number of occurrences of each prefix](https://cp-algorithms.com/string/prefix-function.html#toc-tgt-9)
> 
> [The number of different substring in a string](https://cp-algorithms.com/string/prefix-function.html#toc-tgt-10)
> 
> [Compressing a string](https://cp-algorithms.com/string/prefix-function.html#toc-tgt-11)
> 
> [Building an automaton according to the prefix function](https://cp-algorithms.com/string/prefix-function.html#toc-tgt-12)
> 
> [Practice Problems](https://cp-algorithms.com/string/prefix-function.html#toc-tgt-13)
> 
> ## Prefix function definition
> 
> You are given a string s
> 
> s of length nn. The **prefix function** for this string is defined as an array π\\pi of length nn, where π\[i\]\\pi\[i\] is the length of the longest proper prefix of the substring s\[0…i\]s\[0 \\dots i\] which is also a suffix of this substring. A proper prefix of a string is a prefix that is not equal to the string itself. By definition, π\[0\]\=0
> 
> \\pi\[0\] = 0.
> 
> Mathematically the definition of the prefix function can be written as follows:
> 
> π\[i\]\=maxk\=0…ik:s\[0…k−1\]\=s\[i−(k−1)…i\]
> 
> \\pi\[i\] = \\max\_ {k = 0 \\dots i} \\\\{k : s\[0 \\dots k-1\] = s\[i-(k-1) \\dots i\] \\\\}
> 
> For example, prefix function of string "abcabcd" is \[0,0,0,1,2,3,0\]
> 
> \[0, 0, 0, 1, 2, 3, 0\], and prefix function of string "aabaaab" is \[0,1,0,1,2,2,3\]
> 
> \[0, 1, 0, 1, 2, 2, 3\].
> 
> ## Trivial Algorithm
> 
> An algorithm which follows the definition of prefix function exactly is the following:
> 
>     vector<int> prefix_function(string s) {
>         int n = (int)s.length();
>         vector<int> pi(n);
>         for (int i = 0; i < n; i++)
>             for (int k = 0; k <= i; k++)
>                 if (s.substr(0, k) == s.substr(i-k+1, k))
>                     pi[i] = k;
>         return pi;
>     }
>     
> 
> It is easy to see that its complexity is O(n3)
> 
> O(n^3), which has room for improvement.
> 
> ## Efficient Algorithm
> 
> This algorithm was proposed by Knuth and Pratt and independently from them by Morris in 1977. It was used as the main function of a substring search algorithm.
> 
> ### First optimization
> 
> The first important observation is, that the values of the prefix function can only increase by at most one.
> 
> Indeed, otherwise, if π\[i+1\]\>π\[i\]+1
> 
> \\pi\[i + 1\] \\gt \\pi\[i\] + 1, then we can take this suffix ending in position i+1i + 1 with the length π\[i+1\]\\pi\[i + 1\] and remove the last character from it. We end up with a suffix ending in position ii with the length π\[i+1\]−1\\pi\[i + 1\] - 1, which is better than π\[i\]
> 
> \\pi\[i\], i.e. we get a contradiction.
> 
> The following illustration shows this contradiction. The longest proper suffix at position i
> 
> i that also is a prefix is of length 22, and at position i+1i+1 it is of length 44. Therefore the string s0 s1 s2 s3s\_0 ~ s\_1 ~ s\_2 ~ s\_3 is equal to the string si−2 si−1 si si+1s\_{i-2} ~ s\_{i-1} ~ s\_i ~ s\_{i+1}, which means that also the strings s0 s1 s2s\_0 ~ s\_1 ~ s\_2 and si−2 si−1 sis\_{i-2} ~ s\_{i-1} ~ s\_i are equal, therefore π\[i\]\\pi\[i\] has to be 3
> 
> 3.
> 
> s0 s1⏞π\[i\]\=2 s2 s3\_π\[i+1\]\=4 … si−2 si−1 si⏞π\[i\]\=2 si+1\_π\[i+1\]\=4
> 
> \\underbrace{\\overbrace{s\_0 ~ s\_1}^{\\pi\[i\] = 2} ~ s\_2 ~ s\_3}\\\_{\\pi\[i+1\] = 4} ~ \\dots ~ \\underbrace{s\_{i-2} ~ \\overbrace{s\_{i-1} ~ s\_{i}}^{\\pi\[i\] = 2} ~ s\_{i+1}}\\\_{\\pi\[i+1\] = 4}
> 
> Thus when moving to the next position, the value of the prefix function can either increase by one, stay the same, or decrease by some amount. This fact already allows us to reduce the complexity of the algorithm to O(n2)
> 
> O(n^2), because in one step the prefix function can grow at most by one. In total the function can grow at most nn steps, and therefore also only can decrease a total of nn steps. This means we only have to perform O(n)O(n) string comparisons, and reach the complexity O(n2)
> 
> O(n^2).
> 
> ### Second optimization
> 
> Let's go further, we want to get rid of the string comparisons. To accomplish this, we have to use all the information computed in the previous steps.
> 
> So let us compute the value of the prefix function π
> 
> \\pi for i+1i + 1. If s\[i+1\]\=s\[π\[i\]\]s\[i+1\] = s\[\\pi\[i\]\], then we can say with certainty that π\[i+1\]\=π\[i\]+1\\pi\[i+1\] = \\pi\[i\] + 1, since we already know that the suffix at position ii of length π\[i\]\\pi\[i\] is equal to the prefix of length π\[i\]
> 
> \\pi\[i\]. This is illustrated again with an example.
> 
> s0 s1 s2⏞π\[i\] s3⏞s3\=si+1\_π\[i+1\]\=π\[i\]+1 … si−2 si−1 siπ\[i\] si+1⏞s3\=si+1\_π\[i+1\]\=π\[i\]+1
> 
> \\underbrace{\\overbrace{s\_0 ~ s\_1 ~ s\_2}^{\\pi\[i\]} ~ \\overbrace{s\_3}^{s\_3 = s\_{i+1}}}\\\_{\\pi\[i+1\] = \\pi\[i\] + 1} ~ \\dots ~ \\underbrace{\\overbrace{s\_{i-2} ~ s\_{i-1} ~ s\_{i}}^{\\pi\[i\]} ~ \\overbrace{s\_{i+1}}^{s\_3 = s\_{i + 1}}}\\\_{\\pi\[i+1\] = \\pi\[i\] + 1}
> 
> If this is not the case, s\[i+1\]≠s\[π\[i\]\]
> 
> s\[i+1\] \\neq s\[\\pi\[i\]\], then we need to try a shorter string. In order to speed things up, we would like to immediately move to the longest length j<π\[i\]j \\lt \\pi\[i\], such that the prefix property in the position ii holds, i.e. s\[0…j−1\]\=s\[i−j+1…i\]
> 
> s\[0 \\dots j-1\] = s\[i-j+1 \\dots i\]:
> 
> s0 s1⏟\_j s2 s3π\[i\] … si−3 si−2 si−1 si⏟\_jπ\[i\] si+1
> 
> \\overbrace{\\underbrace{s\_0 ~ s\_1}\\\_j ~ s\_2 ~ s\_3}^{\\pi\[i\]} ~ \\dots ~ \\overbrace{s\_{i-3} ~ s\_{i-2} ~ \\underbrace{s\_{i-1} ~ s\_{i}}\\\_j}^{\\pi\[i\]} ~ s\_{i+1}
> 
> Indeed, if we find such a length j
> 
> j, then we again only need to compare the characters s\[i+1\]s\[i+1\] and s\[j\]s\[j\]. If they are equal, then we can assign π\[i+1\]\=j+1\\pi\[i+1\] = j + 1. Otherwise we will need to find the largest value smaller than jj, for which the prefix property holds, and so on. It can happen that this goes until j\=0j = 0. If then s\[i+1\]\=s\[0\]s\[i+1\] = s\[0\], we assign π\[i+1\]\=1\\pi\[i+1\] = 1, and π\[i+1\]\=0
> 
> \\pi\[i+1\] = 0 otherwise.
> 
> So we already have a general scheme of the algorithm. The only question left is how do we effectively find the lengths for j
> 
> j. Let's recap: for the current length jj at the position ii for which the prefix property holds, i.e. s\[0…j−1\]\=s\[i−j+1…i\]s\[0 \\dots j-1\] = s\[i-j+1 \\dots i\], we want to find the greatest k<j
> 
> k \\lt j, for which the prefix property holds.
> 
> s0 s1⏟\_k s2 s3j … si−3 si−2 si−1 si⏟\_kj si+1
> 
> \\overbrace{\\underbrace{s\_0 ~ s\_1}\\\_k ~ s\_2 ~ s\_3}^j ~ \\dots ~ \\overbrace{s\_{i-3} ~ s\_{i-2} ~ \\underbrace{s\_{i-1} ~ s\_{i}}\\\_k}^j ~s\_{i+1}
> 
> The illustration shows, that this has to be the value of π\[j−1\]
> 
> \\pi\[j-1\], which we already calculated earlier.
> 
> ### Final algorithm
> 
> So we finally can build an algorithm that doesn't perform any string comparisons and only performs O(n)
> 
> O(n) actions.
> 
> Here is the final procedure:
> 
> -   We compute the prefix values π\[i\]
> 
> \\pi\[i\] in a loop by iterating from i\=1i = 1 to i\=n−1i = n-1 (π\[0\]\\pi\[0\] just gets assigned with 0-   0).
> -   To calculate the current value π\[i\]
> \\pi\[i\] we set the variable jj denoting the length of the best suffix for i−1i-1. Initially j\=π\[i−1\]-   j = \\pi\[i-1\].
> -   Test if the suffix of length j+1
> j+1 is also a prefix by comparing s\[j\]s\[j\] and s\[i\]s\[i\]. If they are equal then we assign π\[i\]\=j+1\\pi\[i\] = j + 1, otherwise we reduce jj to π\[j−1\]-   \\pi\[j-1\] and repeat this step.
> -   If we have reached the length j\=0
> j = 0 and still don't have a match, then we assign π\[i\]\=0\\pi\[i\] = 0 and go to the next index i+1
> 
> -   i + 1.
> 
> ### Implementation
> 
> The implementation ends up being surprisingly short and expressive.
> 
>     vector<int> prefix_function(string s) {
>         int n = (int)s.length();
>         vector<int> pi(n);
>         for (int i = 1; i < n; i++) {
>             int j = pi[i-1];
>             while (j > 0 && s[i] != s[j])
>                 j = pi[j-1];
>             if (s[i] == s[j])
>                 j++;
>             pi[i] = j;
>         }
>         return pi;
>     }
>     
> 
> This is an **online** algorithm, i.e. it processes the data as it arrives - for example, you can read the string characters one by one and process them immediately, finding the value of prefix function for each next character. The algorithm still requires storing the string itself and the previously calculated values of prefix function, but if we know beforehand the maximum value M
> 
> M the prefix function can take on the string, we can store only M+1
> 
> M+1 first characters of the string and the same number of values of the prefix function.
> 
> ## Applications
> 
> ### Search for a substring in a string. The Knuth-Morris-Pratt algorithm
> 
> The task is the classical application of the prefix function.
> 
> Given a text t
> 
> t and a string ss, we want to find and display the positions of all occurrences of the string ss in the text t
> 
> t.
> 
> For convenience we denote with n
> 
> n the length of the string s and with mm the length of the text t
> 
> t.
> 
> We generate the string s + \\\\# + t
> 
> s + \\\\# + t, where \\\\#\\\\# is a separator that appears neither in ss nor in tt. Let us calculate the prefix function for this string. Now think about the meaning of the values of the prefix function, except for the first n+1n + 1 entries (which belong to the string ss and the separator). By definition the value π\[i\]\\pi\[i\] shows the longest length of a substring ending in position ii that coincides with the prefix. But in our case this is nothing more than the largest block that coincides with ss and ends at position ii. This length cannot be bigger than nn due to the separator. But if equality π\[i\]\=n\\pi\[i\] = n is achieved, then it means that the string ss appears completely in at this position, i.e. it ends at position ii. Just do not forget that the positions are indexed in the string s + \\\\# + t
> 
> s + \\\\# + t.
> 
> Thus if at some position i
> 
> i we have π\[i\]\=n\\pi\[i\] = n, then at the position i−(n+1)−n+1\=i−2ni - (n + 1) - n + 1 = i - 2n in the string tt the string s
> 
> s appears.
> 
> As already mentioned in the description of the prefix function computation, if we know that the prefix values never exceed a certain value, then we do not need to store the entire string and the entire function, but only its beginning. In our case this means that we only need to store the string s + \\\\#
> 
> s + \\\\# and the values of the prefix function for it. We can read one character at a time of the string t
> 
> t and calculate the current value of the prefix function.
> 
> Thus the Knuth-Morris-Pratt algorithm solves the problem in O(n+m)
> 
> O(n + m) time and O(n)
> 
> O(n) memory.
> 
> ### Counting the number of occurrences of each prefix
> 
> Here we discuss two problems at once. Given a string s
> 
> s of length nn. In the first variation of the problem we want to count the number of appearances of each prefix s\[0…i\]s\[0 \\dots i\] in the same string. In the second variation of the problem another string tt is given and we want to count the number of appearances of each prefix s\[0…i\]s\[0 \\dots i\] in t
> 
> t.
> 
> First we solve the first problem. Consider the value of the prefix function π\[i\]
> 
> \\pi\[i\] at a position ii. By definition it means that in position ii the prefix of length π\[i\]\\pi\[i\] of the string ss appears and ends at position ii, and there doesn't exists a longer prefix that follows this definition. At the same time shorter prefixes can end at this position. It is not difficult to see, that we have the same question that we already answered when we computed the prefix function itself: Given a prefix of length jj that is a suffix ending at position ii, what is the next smaller prefix <j\\lt j that is also a suffix ending at position ii. Thus at the position ii ends the prefix of length π\[i\]\\pi\[i\], the prefix of length π\[π\[i\]−1\]\\pi\[\\pi\[i\] - 1\], the prefix π\[π\[π\[i\]−1\]−1\]
> 
> \\pi\[\\pi\[\\pi\[i\] - 1\] - 1\], and so on, until the index becomes zero. Thus we can compute the answer in the following way.
> 
>     vector<int> ans(n + 1);
>     for (int i = 0; i < n; i++)
>         ans[pi[i]]++;
>     for (int i = n-1; i > 0; i--)
>         ans[pi[i-1]] += ans[i];
>     for (int i = 0; i <= n; i++)
>         ans[i]++;
>     
> 
> Here for each value of the prefix function we first count how many times it occurs in the array π
> 
> \\pi, and then compute the final answers: if we know that the length prefix ii appears exactly ans\[i\]\\text{ans}\[i\] times, then this number must be added to the number of occurrences of its longest suffix that is also a prefix. At the end we need to add 1
> 
> 1 to each result, since we also need to count the original prefixes also.
> 
> Now let us consider the second problem. We apply the trick from Knuth-Morris-Pratt: we create the string s + \\\\# + t
> 
> s + \\\\# + t and compute its prefix function. The only differences to the first task is, that we are only interested in the prefix values that relate to the string tt, i.e. π\[i\]\\pi\[i\] for i≥n+1
> 
> i \\ge n + 1. With those values we can perform the exact same computations as in the first task.
> 
> ### The number of different substring in a string
> 
> Given a string s
> 
> s of length n
> 
> n. We want to compute the number of different substrings appearing in it.
> 
> We will solve this problem iteratively. Namely we will learn, knowing the current number of different substrings, how to recompute this count by adding a character to the end.
> 
> So let k
> 
> k be the current number of different substrings in ss, and we add the character cc to the end of ss. Obviously some new substrings ending in c
> 
> c will appear. We want to count these new substrings that didn't appear before.
> 
> We take the string t\=s+c
> 
> t = s + c and reverse it. Now the task is transformed into computing how many prefixes there are that don't appear anywhere else. If we compute the maximal value of the prefix function πmax\\pi\_{\\text{max}} of the reversed string tt, then the longest prefix that appears in ss is πmax
> 
> \\pi\_{\\text{max}} long. Clearly also all prefixes of smaller length appear in it.
> 
> Therefore the number of new substrings appearing when we add a new character c
> 
> c is |s|+1−πmax
> 
> |s| + 1 - \\pi\_{\\text{max}}.
> 
> So for each character appended we can compute the number of new substrings in O(n)
> 
> O(n) times, which gives a time complexity of O(n2)
> 
> O(n^2) in total.
> 
> It is worth noting, that we can also compute the number of different substrings by appending the characters at the beginning, or by deleting characters from the beginning or the end.
> 
> ### Compressing a string
> 
> Given a string s
> 
> s of length nn. We want to find the shortest "compressed" representation of the string, i.e. we want to find a string tt of smallest length such that ss can be represented as a concatenation of one or more copies of t
> 
> t.
> 
> It is clear, that we only need to find the length of t
> 
> t. Knowing the length, the answer to the problem will be the prefix of s
> 
> s with this length.
> 
> Let us compute the prefix function for s
> 
> s. Using the last value of it we define the value k\=n−π\[n−1\]k = n - \\pi\[n - 1\]. We will show, that if kk divides nn, then kk will be the answer, otherwise there doesn't exists an effective compression and the answer is n
> 
> n.
> 
> Let n
> 
> n be divisible by kk. Then the string can be partitioned into blocks of the length kk. By definition of the prefix function, the prefix of length n−kn - k will be equal with its suffix. But this means that the last block is equal to the block before. And the block before has to be equal to the block before it. And so on. As a result, it turns out that all blocks are equal, therefore we can compress the string ss to length k
> 
> k.
> 
> Of course we still need to show that this is actually the optimum. Indeed, if there was a smaller compression than k
> 
> k, than the prefix function at the end would be greater than n−kn - k. Therefore k
> 
> k is really the answer.
> 
> Now let us assume that n
> 
> n is not divisible by kk. We show that this implies that the length of the answer is nn. We prove it by contradiction. Assuming there exists an answer, and the compression has length pp (pp divides nn). Then the last value of the prefix function has to be greater than n−pn - p, i.e. the suffix will partially cover the first block. Now consider the second block of the string. Since the prefix is equal with the suffix, and both the prefix and the suffix cover this block and their displacement relative to each other kk does not divide the block length pp (otherwise kk divides nn), then all the characters of the block have to be identical. But then the string consists of only one character repeated over and over, hence we can compress it to a string of size 11, which gives k\=1k = 1, and kk divides n
> 
> n. Contradiction.
> 
> s0 s1 s2 s3p s4 s5 s6 s7p
> 
> \\overbrace{s\_0 ~ s\_1 ~ s\_2 ~ s\_3}^p ~ \\overbrace{s\_4 ~ s\_5 ~ s\_6 ~ s\_7}^p
> 
> s0 s1 s2 s3 s4 s5 s6p s7π\[7\]\=5
> 
> s\_0 ~ s\_1 ~ s\_2 ~ \\underbrace{\\overbrace{s\_3 ~ s\_4 ~ s\_5 ~ s\_6}^p ~ s\_7}\_{\\pi\[7\] = 5}
> 
> s4\=s3, s5\=s4, s6\=s5, s7\=s6 ⇒ s0\=s1\=s2\=s3
> 
> s\_4 = s\_3, ~ s\_5 = s\_4, ~ s\_6 = s\_5, ~ s\_7 = s\_6 ~ \\Rightarrow ~ s\_0 = s\_1 = s\_2 = s\_3
> 
> ### Building an automaton according to the prefix function
> 
> Let's return to the concatenation to the two strings through a separator, i.e. for the strings s
> 
> s and tt we compute the prefix function for the string s + \\\\# + ts + \\\\# + t. Obviously, since \\\\#\\\\# is a separator, the value of the prefix function will never exceed |s||s|. It follows, that it is sufficient to only store the string s + \\\\#
> 
> s + \\\\# and the values of the prefix function for it, and we can compute the prefix function for all subsequent character on the fly:
> 
> \\underbrace{s\_0 ~ s\_1 ~ \\dots ~ s\_{n-1} ~ \\\\#}\\\_{\\text{need to store}} ~ \\underbrace{t\_0 ~ t\_1 ~ \\dots ~ t\_{m-1}}\\\_{\\text{do not need to store}}
> 
> \\underbrace{s\_0 ~ s\_1 ~ \\dots ~ s\_{n-1} ~ \\\\#}\\\_{\\text{need to store}} ~ \\underbrace{t\_0 ~ t\_1 ~ \\dots ~ t\_{m-1}}\\\_{\\text{do not need to store}}
> 
> Indeed, in such a situation, knowing the next character c∈t
> 
> c \\in t and the value of the prefix function of the previous position is enough information to compute the next value of the prefix function, without using any previous characters of the string t
> 
> t and the value of the prefix function in them.
> 
> In other words, we can construct an **automaton** (a finite state machine): the state in it is the current value of the prefix function, and the transition from one state to another will be performed via the next character.
> 
> Thus, even without having the string t
> 
> t, we can construct such a transition table (oldπ,c)→newπ
> 
> (\\text{old}\_\\pi, c) \\rightarrow \\text{new}\_\\pi using the same algorithm as for calculating the transition table:
> 
>     void compute_automaton(string s, vector<vector<int>>& aut) {
>         s += '#';
>         int n = s.size();
>         vector<int> pi = prefix_function(s);
>         aut.assign(n, vector<int>(26));
>         for (int i = 0; i < n; i++) {
>             for (int c = 0; c < 26; c++) {
>                 int j = i;
>                 while (j > 0 && 'a' + c != s[j])
>                     j = pi[j-1];
>                 if ('a' + c == s[j])
>                     j++;
>                 aut[i][c] = j;
>             }
>         }
>     }
>     
> 
> However in this form the algorithm runs in O(n226)
> 
> O(n^2 26) time for the lowercase letters of the alphabet. Note that we can apply dynamic programming and use the already calculated parts of the table. Whenever we go from the value jj to the value π\[j−1\]\\pi\[j-1\], we actually mean that the transition (j,c)(j, c) leads to the same state as the transition as (π\[j−1\],c)
> 
> (\\pi\[j-1\], c), and this answer is already accurately computed.
> 
>     void compute_automaton(string s, vector<vector<int>>& aut) {
>         s += '#';
>         int n = s.size();
>         vector<int> pi = prefix_function(s);
>         aut.assign(n, vector<int>(26));
>         for (int i = 0; i < n; i++) {
>             for (int c = 0; c < 26; c++) {
>                 if (i > 0 && 'a' + c != s[i])
>                     aut[i][c] = aut[pi[i-1]][c];
>                 else
>                     aut[i][c] = i + ('a' + c == s[i]);
>             }
>         }
>     }
>     
> 
> As a result we construct the automaton in O(n26)
> 
> O(n 26) time.
> 
> When is such a automaton useful? To begin with, remember that we use the prefix function for the string s + \\\\# + t
> 
> s + \\\\# + t and its values mostly for a single purpose: find all occurrences of the string ss in the string t
> 
> t.
> 
> Therefore the most obvious benefit of this automaton is the **acceleration of calculating the prefix function** for the string s + \\\\# + t
> 
> s + \\\\# + t. By building the automaton for s + \\\\#s + \\\\#, we no longer need to store the string s
> 
> s or the values of the prefix function in it. All transitions are already computed in the table.
> 
> But there is a second, less obvious, application. We can use the automaton when the string t
> 
> t is a **gigantic string constructed using some rules**. This can for instance be the Gray strings, or a string formed by a recursive combination of several short strings from the input.
> 
> For completeness we will solve such a problem: given a number k≤105
> 
> k \\le 10^5 and a string ss of length ≤105\\le 10^5. We have to compute the number of occurrences of ss in the k
> 
> k\-th Gray string. Recall that Gray's strings are define in the following way:
> 
> g1g2g3g4\="a"\="aba"\="abacaba"\="abacabadabacaba"
> 
> \\begin{align} g\_1 &= "a"\\\\ g\_2 &= "aba"\\\\ g\_3 &= "abacaba"\\\\ g\_4 &= "abacabadabacaba" \\end{align}
> 
> In such cases even constructing the string t
> 
> t will be impossible, because of its astronomical length. The kk\-th Gray string is 2k−1
> 
> 2^k-1 characters long. However we can calculate the value of the prefix function at the end of the string effectively, by only knowing the value of the prefix function at the start.
> 
> In addition to the automaton itself, we also compute values G\[i\]\[j\]
> 
> G\[i\]\[j\] - the value of the automaton after processing the string gig\_i starting with the state jj. And additionally we compute values K\[i\]\[j\]K\[i\]\[j\] - the number of occurrences of ss in gig\_i, before during the processing of gig\_i starting with the state jj. Actually K\[i\]\[j\]K\[i\]\[j\] is the number of times that the prefix function took the value |s||s| while performing the operations. The answer to the problem will then be K\[k\]\[0\]
> 
> K\[k\]\[0\].
> 
> How can we compute these values? First the basic values are G\[0\]\[j\]\=j
> 
> G\[0\]\[j\] = j and K\[0\]\[j\]\=0K\[0\]\[j\] = 0. And all subsequent values can be calculated from the previous values and using the automaton. To calculate the value for some ii we remember that the string gig\_i consists of gi−1g\_{i-1}, the ii character of the alphabet, and gi−1
> 
> g\_{i-1}. Thus the automaton will go into the state:
> 
> mid\=aut\[G\[i−1\]\[j\]\]\[i\]
> 
> \\text{mid} = \\text{aut}\[G\[i-1\]\[j\]\]\[i\]
> 
> G\[i\]\[j\]\=G\[i−1\]\[mid\]
> 
> G\[i\]\[j\] = G\[i-1\]\[\\text{mid}\]
> 
> The values for K\[i\]\[j\]
> 
> K\[i\]\[j\] can also be easily counted.
> 
> K\[i\]\[j\]\=K\[i−1\]\[j\]+(mid\=\=|s|)+K\[i−1\]\[mid\]
> 
> K\[i\]\[j\] = K\[i-1\]\[j\] + (\\text{mid} == |s|) + K\[i-1\]\[\\text{mid}\]
> 
> So we can solve the problem for Gray strings, and similarly also a huge number of other similar problems. For example the exact same method also solves the following problem: we are given a string s
> 
> s and some patterns tit\_i, each of which is specified as follows: it is a string of ordinary characters, and there might be some recursive insertions of the previous strings of the form tcntkt\_k^{\\text{cnt}}, which means that at this place we have to insert the string tkt\_k cnt
> 
> \\text{cnt} times. An example of such patterns:
> 
> t1t2t3t4\="abdeca"\="abc"+t301+"abd"\=t502+t1001\=t102+t1003
> 
> \\begin{align} t\_1 &= "abdeca"\\\\ t\_2 &= "abc" + t\_1^{30} + "abd"\\\\ t\_3 &= t\_2^{50} + t\_1^{100}\\\\ t\_4 &= t\_2^{10} + t\_3^{100} \\end{align}
> 
> The recursive substitutions blow the string up, so that their lengths can reach the order of 100100
> 
> 100^{100}.
> 
> We have to find the number of times the string s
> 
> s appears in each of the strings.
> 
> The problem can be solved in the same way by constructing the automaton of the prefix function, and then we calculate the transitions in for each pattern by using the previous results.