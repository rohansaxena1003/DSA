### LIS
  **State**
  Let i be the current index and j be the index of the previously selected element.
  Hence, `f(i,j) represents the max additional length of an increasing subsequence that can be formed using elements from indices i to n-1, given the previously selected element is at index j`.

  **Rec rel**
  We have two choices at current index i, take it or skip it.
  > We can take nums[i] if nums[i] > nums[j], 
  hence `take = f(i+1,i) + 1;` // +1 for length increase; j becomes i because now last element becomes nums[i];
  > `skip = f(i+1,j) +0;` // +0 since no increase in length

   > `f(i,j) = max(take, skip)`;

  **Base Case**
  `if(i==n) { return 0;} // f(n,j) = 0;`

  **Final answer**
  f(0,-1);

  **Recursion**
  We will use rec rel and base case to write the code.
  TC: O(2^n);
  SC: O(n); // stack space

  **Memoization**
  We will make dp array of n by n+1 size. Rows/i from 0 to n-1 and columns/j from -1 to n-1.
  This will reduce recurring calls for same i,j.
  TC: O(n*n);
  SC: O(n*n);

  **Tabulation**
  We will make dp array of size n+1 by n+1. 
  Last row(nth row) will be initialized by 0, that is base case dp[n][j] = 0. We will traverse from n-1th row to 0th row and get our dp array. Final answer will be dp[0][0] where 0th column represents j==-1.
  TC: O(n.n);
  SC: O(n.n);
  

### Number of LIS
  **State**
  Let i represent current index and j represent the index of the previous selected element.
  So, `f(i,j) = (L,C)`;
  > where L represents the max number of additional elements that can be selected from indices i to n-1, given that j is the previously selected element.
  > and C represents the different number of ways we can obtain this max Length L from this state.

  **Rec rel**
  At current index i, we have two options: 
  skip or take current element.
  If we skip current element, then skip = f(i+1,j); // current index increases by 1 and j remains the same.
  Next, we can take the current element if j>=0 and nums[i] > nums[i],
  so take = f(i+1,i); // current index increases by 1 and i becomes the previous index.
  Now, when we choose skip, we get two variables Ls, Cs in return representing Length if we skip and count if we skip. Similarly, we get Lt, Ct for take condition.
  We will do Lt = Lt + 1 because we have selected current element and that increases the length Lt by 1, which needs to be reflected.
  So,
      `skip = (Ls, Cs)`
      `take = (1 + Lt, Ct); if(j>=0 && nums[i] > nums[j])`
  > We have 3 cases:
    If (Lt > Ls) return (Lt, Ct);
    If (Lt < Ls) return (Ls, Cs);
    If (Lt = Ls) return (Lt, Ct + Cs);

  **Base Case**
  f(n,j) = (0,1); // we have traversed full array hence there is only 1 way to select zero elements and that is 1 way: to do nothing.

  **Initial Call**
  We will make f(0,-1) as the initial call because the current element is 0th index and previous index is -1, that is none of the element are evaluated yet and all elements are left to judge.

  **Recursion**
  We will use rec rel and base case to write the code.
  TC: O(2^n); // because each recursive state can generate take and skip branches.
  SC: O(n); // stack space

  **Memoization**
  int[][][] dp = new int[n][n + 1][2];
  with the mapping:
  dp[i][j + 1][0] = maximum length for f(i, j)
  dp[i][j + 1][1] = count of that maximum length
  The shifted column again maps j = -1 to column 0.
  Each length will be initialized by -1.
  TC: O(n.n);
  SC: O(n.n);


### Longest common Subsequence
  **State**
  Let i and j be the current index of text1 and text2 respectively.
  So, `state f(i,j) represents the length of the longest common subsequence obtainable from text1[i...n-1] index and text2[j...m-1] index.`

  **Rec rel**
  > If text1[i]==text2[j], then `takeBoth = 1 + f(i+1,j+1)`; // we are doing +1 we have got a common character
  > else `f(i,j) = skip = Math.max(skip1,skip2)` where `skip1 = f(i+1,j)` and `skip2 = f(i,j+1)`;

  **Base Case**
  `if(i==n || j==m) f(n,m) = 0;` // because no characters left to compare in either or both strings

  **Recursion**
  We will use the rec rel and base case to write the recursion code.
  TC: O(2^(n+m));
  SC: O(n+m); // stack space

  **Memoization**
  We will use dp array of size n by m and reduce repeated calls. Initialize this dp array by -1.
  TC: O(n.m);
  SC: O(n.m) + O(n+m) = O(n.m);

  **Tabulation**
  We will use dp array of n+1 by m+1 size.
  We will use i==n || j==m base case to initialize nth row and mth colum by 0.
  Then proceed with n-1th row and go upwards to row 0 and traverse columns from columns m-1 to column 0 because we need j+1th column value of the current row and that is only possible by LtoR traversal.
  TC: O(n.m);
  SC: O(n.m);

  **Space Optimization**
  We will use two 1D arrays since we need only curr and next row data.
  TC:O(n.m);
  SC: O(m);

  **OneArrSpaceOpt**
  > We need curr[j+1] and next[j+1] at the same time. Hence we will keep track of next[j+1] using a variable because it will be overwritten by curr[j+1]. 
  > next[j] will be stored at the time we are at jth index before it's overwritten by takeBoth.
  TC: O(n.m);
  SC: O(m);


### Edit Distance
  **State:**
  Let i be the current index of 1st string and j be the current index of the 2nd string.
  So, f(i,j) = The min number of operations required to convert word1[i...n-1] to word2[j...m-1].

  **Rec rel**
  If word1[i] === word2[j] {
    match = f(i+1,j+1) + 0; // +0 denotes no operation performed
    `f(i,j) = match;`
  } else {
    insert = f(i,j+1) + 1; // j inserted symbolically, +1 denotes one operation performed
    replace = f(i+1,j+1) + 1; // word2[j] replaces word1[i]; hence i+1,j+1; +1 for one operation performed
    delete = f(i+1,j) + 1; // word1[i] deleted; +1 for one operation performed;  
    `f(i,j) = min(insert,replace,delete);`
  }

  **Base case**
  if(i==n) {
    return m-j; // these m-j chars will be inserted to word1
  }
  if(j == m) {
    return n-i; // n-i chars will be deleted from word1
  }

  **recursion**
  We use rec rel and base case to write our code
  TC: O(3^(n+m)); // 3 branches
  SC: O(n+m); // stack space

  **Memoization**
  We will make dp array of n by m size and initialize it by -1. And store every i,j combo in dp array once calculated.
  TC: O(n.m);
  SC: O(n.m) + O(n+m) = O(n.m); // n+m is stack space

  **Tabulation**
  We will make dp array of n+1 by m+1 size. The nth row will initialize with m-j and mth column will initialize with n-i. Then we will traverse from n-1 row to 0th row and column traversal from m-1th row to 0th column.
  Rest implementation remains same.
  TC: O(n.m);
  SC: O(n.m);

  **Space Optimization**
  Since we are depending on curr and next row data, therefore we will eliminate dp array and make two 1D arrays and reduce space complexity.
  TC: O(n.m);
  SC: O(2.m) = O(m);

  **1D space optimization**
  We will use only one array and store overriding cell values of nextJplus1 and nextJ in variables and use them in replace, delete and match cases.
  TC: O(n.m);
  SC: O(m);

  