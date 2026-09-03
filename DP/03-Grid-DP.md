### Unique Path - 1
> **Problem statement** There is a robot on an m x n grid. The robot is initially located at the top-left corner (i.e., grid[0][0]). The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]). The robot can only move either down or right at any point in time.
> Given the two integers m and n, return the number of possible unique paths that the robot can take to reach the bottom-right corner.

The test cases are generated so that the answer will be less than or equal to 2 * 10^9.

**State:** 
`f(i,j) = Total number of ways to reach (i,j) cell from (0,0) cell.`

**Rec rel:** 
`f(i,j) = No. of ways to reach the above cell + no. of ways to reach left cell`
  > So, f(i,j) = f(i-1,j) + f(i,j-1);

**Base case:** 
`f(0,0) = 1;` // because there is only way to reach start and that is to do nothing.
  Also, if recursion reaches case where one of coordinate is negative like f(-1,0) or f(0,-1), we will ignore these cases.
  So, `f(i,j) = 0 if i<0 or j<0`

**Memoization:** 
We will use a dp array of size m.n to store f(i,j). dp arrays value can be initialized by zero.
TC: O(m.n);
SC: O(m.n) + O(m+n);

**Tabulation:** 
Put dp[0][0] = 1; and build up the array.
TC: For recursive code, it is O(2^(m+n)). Memoization and tabulation its O(m.n)
SC: For recursive code, its O(m+n); // stack space
    For memoization, its O(m+n) + O(m.n); // stack space+dp array
    For tabulation, its O(m.n); //dp array
  
**Space Optimization:** 
We will store prev and current row data since we need the prev element of the current row and same column element of the previous row. The previous row will be initialized by 1 since it's the only possible answer. Also, the first element of each current row will be 1. 
TC: O(m.n);
SC reduces to O(n).


### Unique Path 2
  The only extra constrain here is the obstacle cell.
  If obstacleGrid[i][j] == 1 return 0;
  NOTE: One subtle thing to remember is that in tabulation, obstacle cells must explicitly become 0, because paths cannot flow through them. Other than that, the logic is the same.

### Minimum Path Sum
  State: f(i,j) = Min path sum needed to reach cell i,j from start cell 0,0
  Rec rel: f(i,j) = grid[i][j] + min(f(i-1,j), f(i,j-1));
  Base cases: f(0,0) = grid[0][0]; // 1st base case
  If(i<0 || j<0) return +Infinity; // because we need to ignore this in case of min Sum problem
  Memoization, tabulation, space optimization and TC, SC will remain same as unique path 1 and 2. The only difference is that we need to take care of conditions according to this question.

### Triangle
  State: f(i,j) = Min path sum needed to reach bottom row from current cell i,j
  Rec rel: f(i,j) = triangle[i][j] + min(f(i+1,j), f(i+1,j+1));
  Tabulation: This works best for this question. You will have bottom row as it is and move towards 1st row. So, dp[0][0] will be your answer.
  TC: O(n.n), SC: O(n.n)
  NOTE: The main takeaway for Triangle is that unlike the earlier grid questions, bottom-up traversal is particularly natural because each cell depends on the two cells directly below it.
  Recursive, Memoization and Space Optimization done.

### Dungeon Game
  **State:** 
  f(i,j) = Min health required when entering cell i,j to reach the princess kept in cell m-1,n-1 alive.
  Eg. If: f(i,j)=10, that means: The knight must have at least 10 health when entering (i,j) to guarantee that there is some valid route from (i,j) to the princess.
    So: 9 health  → not enough
    10 health → enough
    11 health → also enough
    20 health → also enough
  **Rec rel:** 
  We need to define multiple factors to get the rec rel.
    1. min(f(i+1,j), f(i,j+1)) // This helps us select the path that is better for the knight.
    2. Now, we will subtract dungeon[i][j] from eq 1 because we need the min health required for the knight at i,j cell.
      So, eq2 is: eq1 - dungeon[i][j];
    3. The knight's health at any cell can't be less than 1, therefore
    `f(i,j)` = `max( 1, eq2 )` = max(  1,  min(f(i+1,j),f(i,j+1)) - dungeon[i][j]   )
    Note: Use the following 3 examples to validate
      -> next required = 6, dungeon[i][j] = -3;
      -> next required = 6, dungeon[i][j] = +4;
      -> next required = 6, dungeon[i][j] = +10;
    **Base Cases:**
    1. If f(m-1,n-1) = max( 1, 1-dungeon[m-1][n-1]);
    Use these examples to validate this
    -> dungeon[m-1][n-1] = +5;
    -> dungeon[m-1][n-1] = 0;
    -> dungeon[m-1][n-1] = -8;
    2. If(i>=m || j>=n) return +INF; // We need min in eq1 therefore invalid paths should never be selected. 
    **Recursion**
    We will use the rec rel and base cases to write our code
    TC: O(2^(m+n)), SC: O(m+n); // space is due to recursion stack
    **Memoization**
    We can see that we are giving function call to the same cell multiple times.So we will use dp array to reduce these repeating calls.
    TC: O(m.n), SC: O(m+n) + O(m.n);
    **Tabulation**
    We fill the array from bottom right to top left.
    TC: O(m.n), SC: O(m.n); // stack space saved 
    **Space Optimization**
    dp[i][j] depends only on:
      down  = next[j]
      right = curr[j+1]

    So, use two 1D arrays:
      next[] → next row
      curr[] → current row

    After each row:
      next = curr

    Time: O(m*n), Space: O(n)


### Day 3 — Grid DP Takeaways

* **Grid DP state is usually 2D:** `dp[i][j]` represents the answer associated with cell `(i,j)`.

* **Always define the state first:** clearly decide whether `f(i,j)` means ways, minimum cost, maximum value, or minimum health required.

* **Identify valid transitions:** ask which cells can lead to the current cell, or which cells can be reached next.

* **Recurrence depends on the objective:**

  * Counting paths → add possibilities.
  * Minimum cost → take `min()`.
  * Maximum value → take `max()`.

* **Boundary values depend on the operation:**

  * Counting invalid path → `0`
  * Minimum problem invalid path → `INF`
  * Maximum problem invalid path → `-INF`

* **Base cases must match the state definition.**

* **Memoization reduces repeated 2D states:** usually from exponential recursion to `O(m*n)`.

* **Tabulation direction depends on dependencies:** fill cells only after the states they depend on are available.

* **Top-left dependency:** if `(i,j)` depends on above/left, fill from top-left toward bottom-right.

* **Bottom-right dependency:** if `(i,j)` depends on down/right, fill from bottom-right toward top-left.

* **Space optimization is possible when only one adjacent row is needed:** replace the `m × n` DP table with 1D `prev/next` and `curr` arrays.

* **Grid DP space can often be reduced:** `O(m*n) → O(n)`.

* **Unique Paths:** count paths.

* **Unique Paths II:** Unique Paths + obstacle handling.

* **Minimum Path Sum:** choose the minimum-cost previous path.

* **Triangle:** state direction can be chosen either top-down or bottom-up; bottom-up is often cleaner.

* **Dungeon Game:** final health is not important; minimize the **initial health required to survive every point of the path**.

* **Most important Day 3 skill:** look at a grid and determine **state + dependency direction + recurrence + base case** before writing code.
  