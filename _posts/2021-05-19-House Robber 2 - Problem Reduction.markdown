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
Important observation: Because the first and last houses are adjacent, we only have two options. The first option is that we consider choosing the first and ignore the last, or we consider choosing the last and ignore the first (recall in House Robber 1: if we pick a house, we no longer consider adjacent houses). Either way, we of course will consider all elements in between as well.  

Let's rephrase the two options. For the first option, since we ignore the last house, our answer can be found by finding the maximum total value when considering the subarray [2:n] (I will use 1-indexing in this article). For the second option, since we ignore the first house, our answer can be found by finding the maximum value when considering the subarray [1:n-1].

Side Note
--------------
It's important to realize that the solution to the subproblem considering the first n-1 elements will not always pick the first element. Likewise, the solution to the subproblem considering the last n-1 elements will not always pick the last element. Indeed, the overall solution may exclude both. This may not sit right with the reader as it feels incorrect. Why exclude both? Well, that happens when you pick the second element and the second to last element. Let's complicate our solution a little bit to remedy our skepticism.


Let's boil down the 3 possible scenarios:
1. The optimal solution involves choosing the first house, in which we ignore the last house. This solution is achieved with our subproblem considering the first n-1 houses.
2. The optimal solution involves choosing the last house, in which we ignore the first house. This solution is the solution to our subproblem considering the last n-1 houses.
3. A solution that chooses neither the first or last house. **This solution is achieved by either of the solutions in 1 and 2** This means the solution can be found by only considering the subarray[2:n-1], which is contained in the first 2 solutions

So rest assured, our two options will get the optimal solution in every scenario. 

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