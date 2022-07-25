# Data-Structure
1. [Strategic Approach to Dp](#strategic-approach-to-dp)
2. [Common Patterns In Dp](#common-patterns-in-dp)
3. [Common Patterns Continued](#common-patterns-continued)
4. [Dp for paths in matrix](#dp-for-paths-in-matrix)
5. [More Practice Problems](#more-practice-problems)
***
## Strategic Approach to Dp
1. [Framework for DP Problems](#framework-for-dP-problems)
2. [Example 198. House Robber](#example-198.-house-robber)
3. [Multidimensional DP](#multidimensional-dP)
4. [Example 1770. Maximum Score from Performing Multiplication Operations](#example-1770.-maximum-score-from-performing-multiplication-operations)
5. [Time and Space Complexity](#time-and-space-complexity)
6. [Chapter 2 quiz](#chapter-2-quiz)
### Framework for DP Problems
In this section, we're going to talk about a framework for solving DP problems. This framework is applicable to nearly every DP problem and provides a clear step-by-step approach to developing DP algorithms.

> For this article's explanation, we're going to use the problem [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/) as an example, with a top-down (recursive) implementation. Take a moment to read the problem description and understand what the problem is asking.

Before we start, we need to first define a term: **state**. In a DP problem, a state is a set of variables that can sufficiently describe a scenario. These variables are called state variables, and we only care about relevant ones. For example, to describe every scenario in Climbing Stairs, there is only 1 relevant state variable, the current step we are on. We can denote this with an integer \text{i}i. If \text{i = 6}i = 6, that means that we are describing the state of being on the 6th step. Every unique value of \text{i}i represents a unique state.

> You might be wondering what "relevant" means here. Picture this problem in real life: you are on a set of stairs, and you want to know how many ways there are to climb to say, the 10th step. We're definitely interested in what step you're currently standing on. However, we aren't interested in what color your socks are. You could certainly include sock color as a state variable. Standing on the 8th step wearing green socks is a different state than standing on the 8th step wearing red socks. However, changing the color of your socks will not change the number of ways to reach the 10th step from your current position. Thus the color of your socks is an **irrelevant** variable. In terms of figuring out how many ways there are to climb the set of stairs, the only relevant variable is what stair you are currently on.



#### The Framework
To solve a DP problem, we need to combine 3 things:

1. **A function or data structure that will compute/contain the answer to the problem for every given state.**

For Climbing Stairs, let's say we have an function \text{dp}dp where \text{dp(i)}dp(i) returns the number of ways to climb to the i^{th}i 
th
  step. Solving the original problem would be as easy as \text{return dp(n)}return dp(n).

How did we decide on the design of the function? The problem is asking "How many distinct ways can you climb to the top?", so we decide that the function will represent how many distinct ways you can climb to a certain step - literally the original problem, but generalized for a given state.

> Typically, top-down is implemented with a recursive function and hash map, whereas bottom-up is implemented with nested for loops and an array. When designing this function or array, we also need to decide on state variables to pass as arguments. This problem is very simple, so all we need to describe a state is to know what step we are currently on \text{i}i. We'll see later that other problems have more complex states.

2. **A recurrence relation to transition between states.**

A recurrence relation is an equation that relates different states with each other. Let's say that we needed to find how many ways we can climb to the 30th stair. Well, the problem states that we are allowed to take either 1 or 2 steps at a time. Logically, that means to climb to the 30th stair, we arrived from either the 28th or 29th stair. Therefore, the number of ways we can climb to the 30th stair is equal to the number of ways we can climb to the 28th stair plus the number of ways we can climb to the 29th stair.

The problem is, we don't know how many ways there are to climb to the 28th or 29th stair. However, we can use the logic from above to define a recurrence relation. In this case, \text{dp(i) = dp(i - 1) + dp(i - 2)}dp(i) = dp(i - 1) + dp(i - 2). As you can see, information about some states gives us information about other states.

> Upon careful inspection, we can see that this problem is actually the Fibonacci sequence in disguise! This is a very simple recurrence relation - typically, finding the recurrence relation is the most difficult part of solving a DP problem. We'll see later how some recurrence relations are much more complicated, and talk through how to derive them.

3. **Base cases, so that our recurrence relation doesn't go on infinitely.**

The equation \text{dp(i) = dp(i - 1) + dp(i - 2)}dp(i) = dp(i - 1) + dp(i - 2) on its own will continue forever to negative infinity. We need base cases so that the function will eventually return an actual number.

Finding the base cases is often the easiest part of solving a DP problem, and just involves a little bit of logical thinking. When coming up with the base case(s) ask yourself: What state(s) can I find the answer to without using dynamic programming? In this example, we can reason that there is only 1 way to climb to the first stair (1 step once), and there are 2 ways to climb to the second stair (1 step twice and 2 steps once). Therefore, our base cases are \text{dp(1) = 1}dp(1) = 1 and \text{dp(2) = 2}dp(2) = 2.

> We said above that we don't know how many ways there are to climb to the 28th and 29th stairs. However, using these base cases and the recurrence relation from step 2, we can figure out how many ways there are to climb to the 3rd stair. With that information, we can find out how many ways there are to climb to the 4th stair, and so on. Eventually, we will know how many ways there are to climb to the 28th and 29th stairs.


#### Example Implementations
Here is a basic top-down implementation using the 3 components from the framework:

<p align="center">
  <img src="images/code.jpeg" align = "center" width="500" height="200" />
</p>
Do you notice something missing from the code? We haven't memoized anything! The code above has a time complexity of O(2^n)O(2 
n
 ) because every call to \text{dp}dp creates 2 more calls to \text{dp}dp. If we wanted to find how many ways there are to climb to the 250th step, the number of operations we would have to do is approximately equal to the number of atoms in the universe.

In fact, without the memoization, this isn't actually dynamic programming - it's just basic recursion. Only after we optimize our solution by adding memoization to avoid repeated computations can it be called DP. As explained in chapter 1, memoization means caching results from function calls and then referring to those results in the future instead of recalculating them. This is usually done with a hashmap or an array.

<p align="center">
  <img src="images/Image 7-24-22 at 11.46 PM.jpg" align = "center" width="600" height="300" />
</p>

With memoization, our time complexity drops to O(n)O(n) - astronomically better, literally.

> You may notice that a hashmap is overkill for caching here, and an array can be used instead. This is true, but using a hashmap isn't necessarily bad practice as some DP problems will require one, and they're hassle-free to use as you don't need to worry about sizing an array correctly. Furthermore, when using top-down DP, some problems do not require us to solve every single subproblem, in which case an array may use more memory than a hashmap.

We just talked a whole lot about top-down, but what about bottom-up? Everything is pretty much the same, except we will start from our base cases and iterate up to our final answer. As stated before, bottom-up implementations usually use an array, so we will use an array \text{dp}dp where \text{dp[i]}dp[i] represents the number of ways to climb to the i^{th}i 
th step.

<p align="center">
  <img src="images/Image 7-24-22 at 11.50 PM.jpg" align = "center" width="600" height="300" />
</p>

> Notice that the implementation still follows the framework exactly - the framework holds for both top-down and bottom-up implementations.


#### To Summarize
With DP problems, we can use logical thinking to find the answer to the original problem for certain inputs, in this case we reason that there is 1 way to climb to the first stair and 2 ways to climb to the second stair. We can then use a recurrence relation to find the answer to the original problem for any state, in this case for any stair number. Finding the recurrence relation involves thinking about how moving from one state to another changes the answer to the problem.

This is the essence of dynamic programming. Here's a quick animation for Climbing Stairs:

<p align="center">
  <img src="images/BeFunky-collage.jpg" align = "center" />
</p>

### Example 198. House Robber

> This is the first of 6 articles where we will use a framework to work through example DP problems. The framework provides a blueprint to solve DP problems, but when you are just starting to learn DP, deriving some of the logic yourself may be difficult. The objective of these articles is to talk through how to use the framework to work through each problem, and our goal is that, by the end of this, you will be able to independently tackle most DP problems using this framework.

In this article, we will be looking at the [House Robber](https://leetcode.com/problems/house-robber/) problem. In an earlier section of this explore card, we talked about how House Robber fits the characteristics of a DP problem. It's asking for the maximum of something, and our current decisions will affect which options are available for our future decisions. Let's see how we can use the framework to develop an algorithm for this problem.

1. A **function or array** that answers the problem for a given state

First, we need to decide on state variables. As a reminder, state variables should be fully capable of describing a scenario. Imagine if you had this scenario in real life - you're a robber and you have a lineup of houses. If you are at one of the houses, the only variable you would need to describe your situation is an integer - the index of the house you are currently at. Therefore, the only state variable is an integer, say i, that indicates the index of a house.

> If the problem had an added constraint such as "you are only allowed to rob up to k houses", then \text{k}k would be another necessary state variable. This is because being at, say house 4 with 3 robberies left is different than being at house 4 with 5 robberies left.

> You may be wondering - why don't we include a state variable that is a boolean indicating if we robbed the previous house or not? We certainly could include this state variable, but we can develop our recurrence relation in a way that makes it unnecessary. Building an intuition for this is difficult at first, but it becomes easier with practice.

The problem is asking for "the maximum amount of money you can rob". Therefore, we would use either a function \text{dp(i)}dp(i) that returns the maximum amount of money you can rob up to and including house \text{i}i, or an array \text{dp}dp where \text{dp[i]}dp[i] represents the maximum amount of money you can rob up to and including house \text{i}i.

This means that after all the subproblems have been solved, \text{dp[i]}dp[i] and \text{dp(i)}dp(i) both return the answer to the original problem for the subarray of \text{nums}nums that spans 00 to \text{i}i inclusive. To solve the original problem, we will just need to return \text{dp[nums.length - 1]}dp[nums.length - 1] or \text{dp(nums.length - 1)}dp(nums.length - 1), depending if we do bottom-up or top-down.

2. A **recurrence relation** to transition between states

> For this part, let's assume we are using a top-down (recursive function) approach. Note that the top-down approach is closer to our natural way of thinking and it is generally easier to think of the recurrence relation if we start with a top-down approach.

Next, we need to find a recurrence relation, which is typically the hardest part of the problem. For any recurrence relation, a good place to start is to think about a general state (in this case, let's say we're at the house at index \text{i}i), and use information from the problem description to think about how other states relate to the current one.

If we are at some house, logically, we have 2 options: we can choose to rob this house, or we can choose to not rob this house.
  1. If we decide not to rob the house, then we don't gain any money. Whatever money we had from the previous house is how much money we will have at this house - which is \text{dp(i - 1)}dp(i - 1).
  2. If we decide to rob the house, then we gain \text{nums[i]}nums[i] money. However, this is only possible if we did not rob the previous house. This means the money we had when arriving at this house is the money we had from the previous house without robbing it, which would be however much money we had 2 houses ago, \text{dp(i - 2)}dp(i - 2). After robbing the current house, we will have \text{dp(i - 2) + nums[i]}dp(i - 2) + nums[i] money.
From these two options, we always want to pick the one that gives us maximum profits. Putting it together, we have our recurrence relation: \text{dp(i)} = \max(\text{dp(i - 1), dp(i - 2) + nums[i]})dp(i)=max(dp(i - 1), dp(i - 2) + nums[i]) .

3. **Base cases**

The last thing we need is base cases so that our recurrence relation knows when to stop. The base cases are often found from clues in the problem description or found using logical thinking. In this problem, if there is only one house, then the most money we can make is by robbing the house (the alternative is to not rob the house). If there are only two houses, then the most money we can make is by robbing the house with more money (since we have to choose between them). Therefore, our base cases are:

  1. \text{dp(0) = nums[0]}dp(0) = nums[0]
  2. \text{dp(1)} = \max( \text{nums[0], nums[1]})dp(1)=max(nums[0], nums[1])


#### Top-down Implementation
Now that we have established all 3 parts of the framework, let's put it together for the final result. Remember: we need to memoize the function!

<p align="center">
  <img src="images/house robber top-down.jpg" align = "center" width="600" height="300" />
</p>


#### Bottom-up Implementation
Here's the bottom-up approach: everything is the same, except that we use an array instead of a hash map and we iterate using a for-loop instead of using recursion.

<p align="center">
  <img src="images/house robber bottom-up.jpg" align = "center"  width="600" height="300"/>
</p>

For both implementations, the time and space complexity is O(n)O(n). We'll talk about time and space complexity of DP algorithms in depth at the end of this chapter. Here's an animation that shows the algorithm in action:

<p align="center">
  <img src="images/house robber animation.jpg" align = "center" />
</p>






### Multidimensional DP


The dimensions of a DP algorithm refer to the number of state variables used to define each state. So far all the algorithms we have looked at required only one state variable - therefore they are one-dimensional. In this section, we're going to talk about problems that require multiple dimensions.
Typically, the more dimensions a DP problem has, the more difficult it is to solve. Two-dimensional problems are common, and sometimes a problem might even require five dimensions. The good news is, the framework works regardless of the number of dimensions.
The following are common things to look out for in DP problems that require a state variable:

1. An index along with some input. This is usually used if an input is given as an array or string. This has been the sole state variable for all the problems that we've looked at so far, and it has represented the answer to the problem if the input was considered only up to that index - for example, if the input is 
nums = [0, 1, 2, 3, 4, 5, 6], then dp(4) would represent the answer to the problem for the input nums = [0, 1, 2, 3, 4].
2. A second index along with some input. Sometimes, you need two index state variables, say i and j. In some questions, these variables represent the answer to the original problem if you considered the input starting at index i and ending at index j. Using the same example above, dp(1, 3) would solve the problem for the input nums = [1, 2, 3], if the original input was [0, 1, 2, 3, 4, 5, 6].
3. Explicit numerical constraints given in the problem. For example, "you are only allowed to complete k transactions", or "you are allowed to break up to k obstacles", etc.
4. Variables that describe statuses in a given state. For example "true if currently holding a key, false if not", "currently holding k packages" etc.
5. Some sort of data like a tuple or bitmask used to indicate things being "visited" or "used". For example, "bitmask is a mask where the i^{th}bit indicates if the i^{th} city has been visited". Note that mutable data structures like arrays cannot be used - typically, only immutable data structures like numbers and strings can be hashed, and therefore memoized.

Multi-dimensional problems make us think harder about deciding what our function or array will represent, as well as what the recurrence relation should look like. In the next article, we'll walk through another example using the framework with a 2D DP problem.

### Top-down to Bottom-up

As we've said in the previous chapter, usually a top-down algorithm is easier to implement than the equivalent bottom-up algorithm. With that being said, it is useful to know how to take a completed top-down algorithm and convert it to bottom-up. There's a number of reasons for this: first, in an interview, if you solve a problem with top-down, you may be asked to rewrite your solution in an iterative manner (using bottom-up) instead. Second, as we mentioned before, bottom-up usually is more efficient than top-down in terms of runtime.

#### Steps to convert top-down into bottom-up

1. Start with a completed top-down implementation.

2. Initialize an array dp that is sized according to your state variables. For example, let's say the input to the problem was an array nums and an integer k that represents the maximum number of actions allowed. Your array dp would be 2D with one dimension of length nums.length and the other of length k. The values should be initialized as some default value opposite of what the problem is asking for. For example, if the problem is asking for the maximum of something, set the values to negative infinity. If it is asking for the minimum of something, set the values to infinity.

3. Set your base cases, same as the ones you are using in your top-down function. Recall in House Robber, 
dp(0) = nums[0] and dp(1) = max(nums[0], nums[1]). In bottom-up, dp[0] = nums[0] and dp[1] = max(nums[0], nums[1]).

4. Write a for-loop(s) that iterate over your state variables. If you have multiple state variables, you will need nested for-loops. These loops should start iterating from the base cases.

5. Now, each iteration of the inner-most loop represents a given state, and is equivalent to a function call to the same state in top-down. Copy the logic from your function into the for-loop and change the function calls to accessing your array. All dp(...) changes into dp[...].

We're done! dp is now an array populated with the answer to the original problem for all possible states. Return the answer to the original problem, by changing return dp(...) to return dp[...].

Let's try a quick example using the House Robber code from before. Here's a completed top-down solution:

<p align="center">
  <img src="images/Screen Shot 2022-07-25 at 10.51.29 AM.png" align = "center"  width="600" height="300"/>
</p>

First, we initialize an array dp sized according to our state variables. Our only state variable is 
i which can take n values.

<p align="center">
  <img src="images/Screen Shot 2022-07-25 at 10.51.40 AM.png" align = "center"  width="600" height="100"/>
</p>


Second, we should set our base cases. dp[0] = nums[0] and dp[1] = max(nums[0], nums[1]). To avoid index out of bounds, we should also just return 
nums[0] if theres only one house.

<p align="center">
  <img src="images/Screen Shot 2022-07-25 at 10.51.48 AM.png" align = "center"  width="600" height="250"/>
</p>

Next, write a for-loop to iterate over the state variables, starting from the base cases.
<p align="center">
  <img src="images/Screen Shot 2022-07-25 at 10.52.06 AM.png" align = "center"  width="600" height="250"/>
</p>


Lastly, copy the recurrence relation over from the top-down solution and put it in the for-loop. Return 
dp[n - 1].
<p align="center">
  <img src="images/Screen Shot 2022-07-25 at 10.52.15 AM.png" align = "center"  width="600" height="250"/>
</p>

###  Example 1770. Maximum Score from Performing Multiplication Operations

> For this problem, we will again start by looking at a top-down approach.
In this article, we're going to be looking at the problem [Maximum Score from Performing Multiplication Operations](https://leetcode.com/problems/maximum-score-from-performing-multiplication-operations/). We can tell this is a DP problem because it is asking for a maximum score, and every time we choose to use a number from nums, it affects all future possibilities. Let's solve this problem with the framework:

1. A function or array that answers the problem for a given state

> In the following discussion, we will use 0-index, since it is more convienient for thinking and coding.
Since we're doing top-down, we need to decide on two things for our function dp. What state variables we need to pass to it, and what it will return. We are given two input arrays: nums and multipliers. The problem says we need to do m operations, and on the ith operation, we gain score equal to multipliers[i] times a number from either the left or right end of nums, which we remove after the operation. That means we need to know 3 things for each operation:

  1. How many operations have we done so far; this tells us what number from multipliers we will be using?
  2. The index of the leftmost number remaining in nums.
  3. The index of the rightmost number remaining in nums.
We can use one state variable, i, to indicate how many operations we have done so far, which means multipliers[i] is the current multiplier to be used. For the leftmost number remaining in nums, we can use another state variable, left, that indicates how many left operations we have done so far. If we have done, say 3 left operations, if we were to do another left operation we would use nums[3]. We can say the same thing for the rightmost remaining number - let's use a state variable right that indicates how many right operations we have done so far.

It may seem like we need all 3 of these state variables, but we can formulate an equation for one of them using the other two. If we know how many elements we have picked from the leftside, left, and we know how many elements we have picked in total,i, then we know that we must have picked i - left elements from the rightside. The original length of nums is n, which means the index of the rightmost element is right = n - 1 - (i - left). Therefore, we only need 2 state variables: i and left, and we can calculate right inside the function.

Now that we have our state variables, what should our function return? The problem is asking for the maximum score from some number of operations, so let's have our function dp(i, left) return the maximum possible score if we have already done i total operations and used left numbers from the left side. To answer the original problem, we should return dp(0, 0).

<p align="center">
  <img src="images/Screen Shot 2022-07-25 at 11.03.18 AM.png" align = "center"  width="600" height="250"/>
</p>

<p align="center">
  <img src="images/Screen Shot 2022-07-25 at 11.03.26 AM.png" align = "center"  width="600" height="250"/>
</p>

<p align="center">
  <img src="images/Screen Shot 2022-07-25 at 11.03.35 AM.png" align = "center"  width="600" height="250"/>
</p>

2. A recurrence relation to transition between states

At each state, we have to perform an operation. As stated in the problem description, we need to decide whether to take from the left end (
nums[left]) or the right end (nums[right]) of the current nums. Then we need to multiply the number we choose by multipliers[i], add this value to our score, and finally remove the number we chose from nums. For implementation purposes, "removing" a number from nums means incrementing our state variables 
i and left so that they point to the next two left and right numbers.

Let mult=multipliers[i] and right = nums.length - 1 - (i - left). The only decision we have to make is whether to take from the left or right of nums.

* If we choose left, we gain mult⋅nums[left] points from this operation. Then, the next operation will occur at (i + 1, left + 1). i gets incremented at every operation because it represents how many operations we have done, and left gets incremented because it represents how many left operations we have done. Therefore, our total score is mult⋅nums[left] + dp(i + 1, left + 1).
* If we choose right, we gain mult⋅nums[right] points from this operation. Then, the next operation will occur at (i + 1, left). Therefore, our total score is mult⋅nums[right] + dp(i + 1, left).
Since we want to maximize our score, we should choose the side that gives more points. This gives us our recurrence relation:

dp(i, left)=max(mult⋅nums[left]+dp(i + 1, left + 1), mult⋅nums[right]+dp(i + 1, left))

Where mult⋅nums[left]+dp(i + 1, left + 1) represents the points we gain by taking from the left end of nums plus the maximum points we can get from the remaining nums array and mult⋅nums[right]+dp(i + 1, left) represents the points we gain by taking from the right end of nums plus the maximum points we can get from the remaining nums array.

3. Base cases

The problem statement says that we need to perform m operations. When i equals m, that means we have no operations left. Therefore, we should return 0.



#### Top-down Implementation

Let's put the 3 parts of the framework together for a solution to the problem.

Protip: for Python, the [functools](https://docs.python.org/3/library/functools.html) module provides super handy tools that automatically memoize a function for us. We're going to use the @lru_cache decorator in the Python implementation.

> If you find yourself needing to memoize a function in an interview and you're using Python, check with your interviewer if using modules like functools is OK.
This particular problem happens to have very tight time limits. For Java, instead of using a hashmap for the memoization, we will use a 2D array. For Python, we're going to limit our cache size to 2000.

<p align="center">
  <img src="images/BeFunky-collage (2).jpg" align = "center"  width="800" height="500"/>
</p>


#### Bottom-up Implementation

In the bottom-up implementation, the array works the same way as the function from top-down. dp[i][left] represents the max score possible if i operations have been performed and left operations have been performed.

Earlier in the explore card, we learned that while bottom-up is typically faster than top-down, it is often harder to implement. This is because the order in which we iterate needs to be precise. You'll see in the implementations below that we use the same math to calculate right, and the same recurrence relation but we need to iterate backwards starting from m (because the base case happens when i equals m). We also need to initialize dp with one extra row so that we don't go out of bounds in the first iteration of the outer loop.

<p align="center">
  <img src="images/Screen Shot 2022-07-25 at 11.11.04 AM.png" align = "center"  width="600" height="300"/>
</p>

The time and space complexity of both implementations is O(m^2) where m is the length of multipliers. We will talk about more in depth about time and space complexity at the end of this chapter.



## Common Patterns In Dp
## Common Patterns Continued
## Dp for paths in matrix
## More Practice Problems
