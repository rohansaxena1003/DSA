### Unique Path - 1
  State: `f(i,j)` = Total number of ways to reach (i,j) cell from (0,0) cell.
  Rec rel: `f(i,j)` = No. of ways to reach the above cell + no. of ways to reach left cell
  So, f(i,j) = f(i-1,j) + f(i,j-1);
  Base case: f(0,0) = 1;// because there is only way to reach start and that is to do nothing.
  Also, if recursion reaches case where one of coordinate is negative like f(-1,0) or f(0,-1), we will ignore these cases.
  So, f(i,j) = 0 if i<0 or j<0
  Memoization: We will use a dp array of size m.n to store f(i,j). dp arrays value can be initialized by zero.
  Tabulation: Put dp[0][0] = 1; and build up the array.
  TC: For recursive code, it is O(2^(m+n)). Memoization and tabulation its O(m.n)
  SC: For recursive code, its O(m+n); // stack space
      For memoization, its O(m+n) + O(m.n); // stack space+dp array
      For tabulation, its O(m.n); //dp array
  Space Optimization: We will store prev and current row data since we need the prev element of the current row and same column element of the previous row. The previous row will be initialized by 1 since it's the only possible answer. Also, the first element of each current row will be 1. SC reduces to O(n).

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

 


  