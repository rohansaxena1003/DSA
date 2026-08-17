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
