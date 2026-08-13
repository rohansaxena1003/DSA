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


  
