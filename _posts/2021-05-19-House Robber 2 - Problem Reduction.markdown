---
layout: post
title:  House Robber 2 - Problem Reduction
date:   2021-05-19 13:29:00 -0500
categories: lc algorithms house-robber
---

Introduction
--------------
I like House Robber 2 because we do not have to do much to get the solution. It showcases a cool example of problem reduction - reducing a problem down to another problem. In problem reduction, if you have the solution too the equivalent problem, you also have the solution to the original problem! So the only challenge here is recognizing the problem is similar to one you have done before (in this case, it's House Robber 1!) 

The problem specification: The objective, constraints, and input are the same as the ones in House Robber 1. The difference is that the houses are arranged in a circle, meaning the first house is next to the second house...and that's the only difference.

Original problem can be found [here](https://leetcode.com/problems/house-robber-ii/){:target="_blank"}

Recognition
--------------
Important observation: Because the first and last houses are adjacent, we only have two options. The first option is that we consider choosing the first and ignore the last, or we consider choosing the last and ignore the first (recall in House Robber 1: if we pick a house, we no longer consider adjacent houses). Either way, we of course will consider all elements in between aas well.  

Let's rephrase the two options. For the first option, since we ignore the last house, our answer can be found by finding the maximum total value when considering the first n-1 elements. For the second option, since we ignore the first house, our answer can be found by finding the maximum value when considering the last n-1 elements. Sound familiar? The solution to each option is just running House Robber 1 on a subarray. 

Algorithm
---------------
The algorithm kind of follows immediately from recognition - you can just run your solution to House Robber 2 two times, the only thing that changes is input. You can then return the best of the two options


Code
--------------
My solution:  

```
class Solution {
public int rob(int[] nums) {
    if(nums.length ==1) return nums[0];
	return Math.max(originalRobberProblem(Arrays.copyOfRange(nums,0,nums.length-1)), originalRobberProblem(Arrays.copyOfRange(nums,1,nums.length)));

}

public int originalRobberProblem(int[] nums){
    int prevBest = 0;
	int currentBest = 0;
	for(int num : nums){
		//choice of taking num
		int temp = currentBest;
		currentBest = Math.max(num + prevBest, currentBest);
		prevBest = temp;
	}
	return currentBest;



}
}
```