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