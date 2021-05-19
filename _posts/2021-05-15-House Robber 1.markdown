---
layout: post
title:  House Robber 1 - Classic DP
date:   2021-05-15 22:20:00 -0500
categories: lc algorithms house-robber
---

Introduction
---------------
The House Robber problem introduces a pretty canonical dynamic programming approach so it's a good starting point to getting comfortable with DP. I'll probably write an introductory article eventually about DP, but this post will asssume some basic understanding. 

The problem specification is pretty straight forward - you have an array of integers, where each number represents a value (presumably representing a house with some value waiting to be robbed?). You, as a robber, need to strategically pick values (rob the houses) such that you ***maximize the total value*** while ***avoid picking two adjacent values***. So in typical fashion, you have an objective and a constraint.

Problem can be found [here](https://leetcode.com/problems/house-robber/){:target="_blank"}

First intuition: Greedy Approach?
-----------------
When I first read this problem after taking an algo class at my uni, I didn't even recognize it as a DP problem (that's usually 50% of the challenge with DP, recognizing it's DP...). my first instinct was a greedy algorithm, where we naively compare two options. The first option would be to pick the even-indexed numbers (numbers at index 0, 2, 4, ...). The second option would be to pick the odd-indexed numbers (numbers at index 1, 3 , 5, ...). After all, these are the only two options that can allow us to pick the maximum number of values, no? Why even ignore a house when you can greedily pick it? 


Problem with Greedy Approach
------------------
Well, maximizing number of values chosen != maximizing total value  

Consider the following array: [100, 1, 1, 100, 1]  

With our first approach, we get a total value of 102. With our second, we also get a total value of 102. But we know the answer is 200 here. 

So what can we do? 


Brief DP Discussion/Detour
--------------------
Again, I'll probably write another post talking about DP more in depth, but two important points/reminders:


1. DP will always involve optimal substructure. This just means the answer to a problem will depend on the answer to the problem's subproblems
2. Because a problem can be broken down into subproblems, we can form some sort of recurrence relation


Algorithm
-------------------
So with those 2 points, let's make an important observaton here:  

Assume we have an instance of the problem, say [5,4,6,7,1,1]. Let's say we already know the maximum value we can get when considering the first 2 elements (5,4), which is 5. Let's say we also already know the maximum value we can get by only considering the first 3 elements (5,4,6), which is 11.  Now, can we quickly find the maximum value we can get by considering the first 4 elements?. After all, we didn't really change the problem all that much right? By considering one more element, there isn't too much burden with overchoice here: ***Really, we have two choices: we can either choose to include 7, or we choose not to include 7.***  

Let's say we choose 7. Well, that just means we cannot pick 6, since it is adjacent to 7. With this restriction, we can reason that ***the best we can do is try to find the maximum total value that ignores the value 6.*** Recall the original problem we're trying to solve is finding the maximum value considering the first 4 elements. Right now, we're choosing 7 (the 4th element) and consequently we are forced to ignore 6 (the 3rd element). So now we must ask: how can we optimally choose the remaining values (the 1st and 2nd element)? After all, if we know the maximum sum for the remaining values, adding 7 this sum will give us the maximum value when considering the first 4 elements and picking the fourth element. The trick is that this maximum sum is already known! Recall that we assumed we already know the maximum value we can get by considering the first 2 elements, so calculating this is really fast!

Let's consider the only other option: not picking 7. If you don't pick 7, you're free to pick 6 if you would like. There's no restriction here. If we don't pick 7, then our answer would just be the maximum total value we can get by considering the rest of the values (5,4,6). Do we know this? Yes! We already know the maximum value we can get when considering the first 3 elements.  

So similar to the greedy approach, we're considering two options and picking the maximumm of the two. The only difference is that our two options are much more informed :)  

Let's tie things up real quick. We now have the solution when we consider the first 4 elements. We also have the solution when considering the first 3 elements. Now, we can repeat the logic above to get the answer for the first 5 elements! You can see that we can repeat this process until we reach the solution when considering all the elements! 


So the recurrence relation should be clear now. More formally, if we know f(i-1) and f(i-2), we can find out f(i) quickly (kind of like fibonacci right?). To clarify, f(i) here would represent the maximum value we can get when considering the first i elements. f(i) would then be the maximum between values[i] + f(i-2) and f(i-1). 
