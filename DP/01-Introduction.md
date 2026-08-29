### Why and when does greedy algorithm fails ?
    A greedy algorithm works when you choose the best option 
    at each step and this gives you the best final answer.
    When this fails, greedy algorithm fails. Eg. You have 3 
    coins {1,4,3} and you want to have 6 rupees by using 
    minimum number of coins. Here, the greedy algorithm will 
    choose a 4 rs coin and then two 1 rs coins{4,1,1}, that is, 
    3 coins in total. But, the best answer here is choosing 
    {3,3} that is 2 coins. So, greedy algorithm fails here.

What is DP ?
    So, DP problems have 2 properties: Optimal substructure 
    and Overlapping subproblems. 

What is memoization ?
    It is the process of storing repeated answers in dp array
    while doing recursion. This greatly reduces stack calls
    and improves time complexity.

What is tabulation ?
    It uses iterative calls to get dp array and this doesn't
    use recursion and it's more efficient than memoization.

Difference between memoization and tabulation.
    ## Memoization vs Tabulation

### Memoization (Top-Down)
- Starts from the original problem and recursively breaks it into smaller subproblems.
- Uses recursion.
- Solves states only when they are needed (on demand).
- Stores computed states to avoid recomputation.
- Requires a recursion stack.
- Easier to derive directly from the recurrence relation.
- Can skip unreachable or unnecessary states.
- Has recursion overhead, so it is usually slightly slower.
- May cause StackOverflowError for very deep recursion.
- Space Complexity: O(DP Array + Recursion Stack).

### Tabulation (Bottom-Up)
- Starts from the base cases and builds up to the final answer.
- Uses iteration (loops).
- Computes states in a predefined order.
- Does not use recursion.
- Does not require a recursion stack.
- Requires identifying the correct order to fill the DP table.
- Usually computes all states, even if some are unnecessary.
- Faster in practice because there is no function call overhead.
- No risk of stack overflow.
- Space Complexity: O(DP Array) (can often be optimized further depending on the dependencies for computing current dp state).

---

## Quick Comparison

| Memoization | Tabulation |
|-------------|------------|
| Top-Down | Bottom-Up |
| Uses Recursion | Uses Loops |
| Solves states on demand | Solves states in a fixed order |
| Uses Recursion Stack | No Recursion Stack |
| Easier to derive | Requires fill-order analysis |
| May skip unnecessary states | Usually computes all states |
| Slightly slower (function call overhead) | Usually faster |
| Risk of StackOverflowError | No StackOverflowError |

---

## Easy Way to Remember

**Memoization asks:**
> "What smaller problems do I need to solve to answer this question?"

**Tabulation says:**
> "I already know the answers to the smallest problems. Let me build the larger answers step by step."

## Problem 1: Fibonacci Series
    State: f(n) = n;
        This means dp[0] = f(0), dp[1] = f(1), ...
    Recurrence relation: f(i) = f(i-1) + f(i-2);
    Memoization: Once you solve f(i), you store it in dp[i] and use it when f(i) is needed.
    Tabulation: We keep dp[0] = 0 and dp[1] = 1 as base cases and move upward.
    Space optimization: Since you need dp[i-1] and dp[i-2] only, you can avoid dp array and use 2 variables only.

### Problem 2: Climbing Stairs
**State** 
    f(i) = No. of distinct ways to reach i from floor, where i is the current step.

**Recurrence Relation:** 
    f(i) = f(i-1) + f(i-2);//We can jump from last or second last step to reach current step.

**Base case:** 
    dp[0] = 1; // there is one way to reach floor and that is to nothing.Also, dp[-1] = 0; // there is no way to reach floor from -1 because we can only jump upwards.

**Memoization:** 
    Once you solve f(i), you store it in dp[i] and use it when f(i) is needed.

**Tabulation:** 
    We keep dp[0] = 1 and dp[-1] = 0 as base cases and move upward.

**Space optimization:** 
    Since you need dp[i-1] and dp[i-2] only, you can avoid dp array and use 2 variables only.

TC: O(2^n), O(n), O(n) and O(n) for recursive, memoization, tabulation and space optimization.
SC: O(n) for recursive, memoization and tabulation. O(1) for space optimization.


## Problem 3: Min Cost of Climbing Stairs
**State:** 
    f(i) = Min cost to reach stair i starting from the stair 0 or stair 1.
    
**Recurrence relation:** 
    At stair i, I have arrived from either stair i-1 or stair i-2. Also, I have to pay cost[i] at stair i in either case.
    Therefore, f(i) = cost[i] + min(f(i-1), f(i-2));

**Base case:** 
    f(0) = cost[0] and f(1) = cost[1]; //You can start from stair 0 or 1

**Recursion**
    We will use base case and recurrence relation to write our code.
    TC: O(2^n);
    SC: O(n); // stack space

**Memoization**
    We will make dp array of size n. Now, we traverse from top and use recursion to get answer. This dp array will reduce recurring calls.
    TC: O(n);
    SC: O(n + n) = O(n);

**Tabulation**
    We will make dp array of size n and initialize dp[0]=cost[0], dp[1]=cost[1] and build our dp array from bottom to top to get our answer.
    TC: O(n);
    SC: O(n);

**Space Opt**
    Since we need only dp[i-1] and dp[i-2] at any time, thus we don't need any array. We can only need only two variables.
    TC: O(n);
    SC: O(1);


## Problem 4: House Robber
    State: f(i) = Max money that can be robbed starting from house i till the last house
            OR
           f(i) = Max money robbed till house i from the start
           Both approaches are correct and are used for memoization and tabulation respectively.
    Recurrence relation: At house i, if I rob it I collect the money from this house and go to house i+2. If I skip it, I go to house i+1. Therefore, 
    f(i) = Math.max(money[i]+f(i+2), f(i+1));
    Memoization: We store f(i+1) and f(i+2) in dp array to reduce recursive calls.
    Tabulation: We store f(i-1) and f(i-2) in dp array to build up the answer.
    Space optimization: Since we only need only next 2/ previous 2 values, we can bypass dp array and save space.
