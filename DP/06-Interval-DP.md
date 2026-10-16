Interval DP
Before solving these problems, you need to understand the central question behind this entire category:
What is the optimal way to solve a problem over a range by dividing that range into smaller ranges?
Suppose an array contains:
[A, B, C, D, E]
Instead of solving the entire array at once, we consider different ways of dividing it:
- [A] | [B, C, D, E]
- [A, B] | [C, D, E]
- [A, B, C] | [D, E]
- And so on.
Each division creates smaller subproblems. We calculate their results and combine them to obtain the answer for the original interval.
The general state is:
\[
\boxed{f(i,j)=\text{optimal answer for the interval }[i,j]}
\]
Here:
- i is the starting index.
- j is the ending index.
- f(i,j) represents the optimal answer for that interval.