### 0/1 KnapSack
  **State:**
  f(index, remainingWeight) means the maximum value that can be attained using items from index onwards, when the knapsack has *remainingWeight capacity* left.
  Eg, if index = 2 then items at index 0 and 1 have been considered, while 2 is yet to be decided.
  **Rec rel:**
  Let i represent index and j represent weight.
  `skip = f(i+1,j);` 
  `take = value[i] + f(i+1,j-weight[i]);` // if j-weight[i] >= 0;
  So, 
  `f(i,j) = max(take, skip) if j-weight[i] >= 0`
  `OR`
  `f(i,j) = skip`;
  **Base Case:**
  if(i==n) return 0; // we have reached the last index and that every item has been considered.
  if(j==0) return 0; // no positive weight can be selected, so no extra value can be gained.
  **Recursion**
  We will use the rec rel and base cases to write our code.
  TC: O(2^n), SC: O(n); // stack space
  **Memoization**
  We can see we will reach f(i,j) multiple times.
  We will use dp array of size n by (W+1) and optimize time complexity.
  TC: O(n.W), SC(n.W);
  **Tabulation**
  We will have dp array of size n+1 rows and W+1 columns because weights can be from 0 to W and nth index row is needed to cater to base case.
  Now, so we can fill the first column with zero, because if the remaining weight is zero, then we cannot add any value.
  Next, we will traverse from n-1th row because to calculate skip or take, we need data from i+1th row. This also means that we can do column traversal LR or RL because it's independent of current column and only needs j-weight[i] column in i+1th row which is already calculated.
  return dp[0][W] as final answer.
  TC: O(n.W), SC: O(n.W);
  **Space Optimization**
  Since we need data only from i+1th row for the current row, we will use two 1D arrays named next and curr to stores data.
  TC:O(n.W), SC: O(2.W) = O(W); // linear space used
  **Single Array space Optimization**
  Look at the conditions to calculate skip, take. We need j-weight[i] column data and since j-weight[i] != j because weight[i] > 0, therefore we will traverse column from W to 0, that is RL. For row, we only need data from i+1th row at any time so we will store it at the same index. 
  This way we will need only single array to obtain answer.
  TC: O(n.W), SC: O(1.W) = O(W); // single array used


### Partition Equal Subset Sum
  **State**
  Let i denote index and j denote remainingSum needed to reach target
  f(i,j) represents weather we can form remainingSum j using some elements from index onwards.
  **Rec rel**
  If we skip element at i then, `skip = f(i+1,j);`
  If we take it, then `take = f(i+1, j-element[i]); // j-element[i] >= 0`
  So, 
  `f(i,j) = take || skip; if j-element[i] >= 0`
  `OR`
  `f(i,j) = skip;`
  **Base Case**
  `if(j==0) return true;` // valid subset found
  `if(i==n) return false;` // we have reached end of array but haven't found a valid subset.
  // check j==0 condition first to avoid f(n,0) error
  **Recursion**
  We will use the rec rel and base cases to write our code.
  TC: O(2^n), SC: O(n); // stack space
  **Memoization**
  We will make a dp array of n.(T+1) size. Initialize it with -1, use 0 for false and 1 for true. We don't use boolean array because if can't determine if dp[i][j] is already determined.
  TC: O(n.T), SC: O(n) + O(n.(T+1)) = O(n.T); // O(n) is stack space
  **Tabulation**
  We will make a DP array of size (n+1).(T+1)
  The first column will be initialized by 1; //base case
  Last column will be initialized by 0; // 2nd base case
  Now, we work from row n-1 to row 0 and calculate each cell.
  Our final answer will be dp[0][T]; // this means T can be formed by using elements from index 0 onwards. 
  Although the answer can also be dp[i][T] == 1, we will use dp[0][T] for clariy.
  TC: O(n.T), SC: O(n.T);
  `We can also use boolean dp array for tabulation`
  **Space Optimization & Single Array space optimization**
  Both follow same pattern as 0/1 knapsack. 
  // we traverse columns from T to 0 because we need j-nums[i] for the previous row and traversing LR will override this data.
  TC: O(n.T), SC: O(n);


### Target Sum
  **State**
  Let `i be current index` and 
  `j be currentSum`, that is, `the accumulated signed sum for elements from 0 to i-1`;
  > the state = f(i,j) tells the number of ways to assign signs to elements from i onwards such that the final accumulated sum becomes targetSum.
  
  **Rec rel**
  `plus = f(i+1, +nums[i] + j)`;
  `minus = f(i+1, -nums[i] + j)`;
  > So, `f(i, currentSum) = plus + minus`;
  
  **Base Case**
  if (index == nums.length) {
    return currentSum == target ? 1 : 0;
  }
  
  **Recursion**
  We use base case and the rec rel to write our recursive code.
  TC: O(2^n); // each element creates 2 branches
  SC: O(n); // we are only incrementing i ... n
  
  **Memoization**
  Let S denote total positive sum of array elements. Since we can assign minus(-) sign to all the elements, therefore minimum sum possible becomes -S. So the range is -S to +S, that is 2S+1 possible currentSum.
  Hence, we use dp array of size n by 2S+1 where n is array indexes from 0 to n-1.
  We initialize the dp array elements by -1 to denote dp[i][j] hasn't been processed yet.
  Now, to denote currentSum correctly in this array, we use an offset of +S with each currentSum.
  > Let S = 5, so 2S+1= 11 is our total columns. If currentSum= -5, we use -5+S=0th column to denote it. If currentSum = -1, we use -1+S=4th column; if currentSum = 5, we use 10th column. 
  The calls remain the same as recursive code. Only the dp access changes in this code.
  TC: O(n.(2S+1)) = O(n.S);
  SC: O(n) + O(n.(2S+1)) = O(n) + O(n.S) = O(n.(S+1)) = O(n.S);
  
  **Tabulation**
  dp array will have n+1 rows from 0 to n and 2S+1 columns from 0 to 2S. 
  dp[n][S + targetSum] = 1; // base case
  Then we start from row n-1 and go till row 0. For each row, we travel columns from -S to +S or vice versa.For each cell , we `calculate plus and minus using plusCol, minusCol and calculate dp[i][j] where j is currentSum + S, and currentSum is the current column.`
  TC: O(n.(2S+1)) = O(n.S);
  SC: O(n.(2S+1)) = O(n.S);
  
  **Space Optimization**
  Since we need data from only `next` row for the current row, we will use two 1D arrays for this purpose.
  TC: O(n.(2S+1)) = O(n.S)
  SC: O(2.n) = O(n);


### Last Stone Weight-2
  **State**
  Let i represent the current index and j represent the absolute difference of 2 'groups' for the already processed stones. The groups represent the two sides of the eventual signed subtraction; they are not literal stones being combined.
  `f(i,j) = The minimum possible stone weight after assigning stones from i to n-1 to either of the two conceptual groups, given that stones from 0 to i-1 have already been assigned and the current absolute difference between their group sums is j.`

  **Rec rel**
  We have two options, wither to assign stones[i] to the larger group or to the smaller group. The new difference that we get after assigning current stone to larger and smaller group is:
  lGroupAssignDiff = j + stones[i]; // you can verify with example
  sGroupAssignDiff = Math.abs(j - stones[i]);
  So, the two function calls are:
  > largerGroupChoice = f(i+1, j + stones[i]);
  > smallerGroupChoice = f(i+1, Math.abs(j - stones[i]));
  Finally, 
  `f(i,j) = min( largerGroupChoice, smallerGroupChoice );`

  **Base Case**
  `f(n,j) = j;` // j represents the final absolute difference that we are gonna get after processing all the stones. Hence we will return j from the base case.

  **Recursion**
  We will use the base case and recurrence relation to write the code for it.
  TC: O(2^n); // two calls inside every call
  SC: O(n); // stack space

  **Memoization**
  We will make dp array of n by S+1 where S is the total sum of all stones. We need S because all stones can belong to one group. Eg. there is only one stone.
  This dp array will greatly reduce the recurring call for same i,j.
  TC: O(n*S);
  SC: O(n*S) + O(n) = O(n*S); // O(n) is stack space

  **Tabulation**
  We need dp array of size n+1 by S+1.
  We fill row n with j values, that is dp[n][j] = j; // base case
  Since we need values from row i+1 for every row therefore we traverse from row n-1 towards row 0 and column traverse order doesn't matter.
  Next, when we are at row i, we need to traverse columns from 0 to prefixSum because:
  > The largest possible difference occurs when every processed stone is placed in the same group: jmax = prefixSum i.
  > The smallest possible difference cannot be negative: jmin = 0
  Therefore, `every reachable j must lie within: 0 ≤ j ≤ prefixSum i`

  Let prefixSum = 0, suffixSum = 0;
  suffixSum at n-1th row = stones[n-1], prefixSum = S - suffixSum;
  Then at row n-2, suffixSum += stones[n-2], prefixSum = S - suffixSum;
  .
  .
  .
  At row i, suffixSum += stones[i], prefixSum = S - suffixSum
  .
  .
  At row 0, suffixSum = S, prefixSum = 0;
  TC: O(n.S);
  SC: O(n.S);

  **Space Optimization**
  Since we need next and curr row at any time, we will use two 1D arrays of S+1 size.
  TC: O(n.S)
  SC: O(S);


# Day 4 — Knapsack and Subset DP Takeaways

* Learned to recognize the common **take/skip pattern** across subset-based DP problems.

* Understood that the second state variable can represent different things:

  * Remaining capacity
  * Remaining target
  * Current accumulated sum
  * Current signed sum
  * Current absolute difference

* Learned that the required result determines how choices are combined:

  * Maximum value → `max`
  * Minimum difference → `min`
  * Possibility → OR
  * Number of ways → addition

* Practised the complete DP conversion:

```text
Recursion → Memoization → Tabulation → Space Optimization
```

* Learned to derive the tabulation base row directly from the recursive base case.

* Understood one-array traversal direction:

  * Lower-column dependency → right to left
  * Higher-column dependency → left to right
  * Dependencies on both sides → direct one-array updating is unsafe

* **0/1 Knapsack:** Learned to maximize value under a capacity constraint and use backward traversal for one-array optimization.

* **Partition Equal Subset Sum:** Learned to transform equal partitioning into checking whether a subset with sum `totalSum / 2` exists.

* **Target Sum:** Learned to track negative and positive sums using offset normalization and reduce space to two arrays.

* **Last Stone Weight II:** Learned to look beyond the physical simulation and represent repeated smashing as the absolute difference between two conceptual groups.

* **Count Subsets:** Learned that counting requires adding take and skip results rather than using Boolean OR.

* Understood why zeroes require special attention in counting problems: taking and skipping a zero are different choices even though the sum remains unchanged.

* Learned the difference between:

  * A range of possible DP columns
  * The states that are genuinely reachable within that range

* Understood that (O(nW)), (O(nK)), and (O(nS)) subset-DP solutions are pseudo-polynomial because they depend on numeric values.

## Most Important Skill Developed

> Identifying what information from previously processed elements must be carried forward, and then expressing that information as the second DP state variable.

## Pending Revision After Day 7

* Target Sum subset transformation and one-array optimization.
* Last Stone Weight II standard minimum subset-difference one-array formulation.

