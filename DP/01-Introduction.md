Why and when does greedy algorithm fails ?
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
    use recursive it's more efficient than memoization.

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
- Space Complexity: O(DP Array) (can often be optimized further).

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

Problem 1: Fibonacci Series
    State: f(n) = n;
        This means dp[0] = f(0), dp[1] = f(1), ...
    Recurrence relation: f(i) = f(i-1) + f(i-2);
    Memoization: Once you solve f(i), you store it in dp[i] and use it when f(i) is needed.
    Tabulation: We keep dp[0] = 0 and dp[1] = 1 as base cases and move upward.
    Space optimization: Since you need dp[i-1] and dp[i-2] only, you can avoid dp array and use 2 variables only.