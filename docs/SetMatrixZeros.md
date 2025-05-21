[https://leetcode.com/problems/set-matrix-zeroes](https://leetcode.com/problems/set-matrix-zeroes)

# Intuition

I was careful enough to notice that the problem must be done ["in place"](https://en.wikipedia.org/wiki/In-place_algorithm), but I overcomplicated the requirements by thinking ONLY the original input matrix could be used (no additional lists could be used).

# Approach

My original approach was to iterate through the matrix until a zero was found. Once a zero was found, I would enter a recursive function which would zero out all adjacent cells in the row & column of the "naturally occurring" zero, *while also* checking for more "naturally occurring" zeros. As usual, **recursive functions rarely seem to be the most efficient answer with these solutions.**

After submitting my solution and surprisingly passing (beating about 5% of people on both time and space), I think realized the stupidly easy optimal solution.

# Optimization

The optimal solution you see below simply iterates through the matrix to find any cells with zero, and records the column and row index, but does not modify the matrix yet.

Next - as a separate step (once all zeros have been found), then the accrued list of column and row indices is used to zero-out the necessary rows and columns. Very simple and no recursion necessary.

# Complexity

- Time complexity:

`O(n)`

- Space complexity:

`O(zr + zc)` where `zr` is the number of rows with zeros and `zc` is the number of columns with zeros

# Code

```
public class Solution
{
    public void SetZeroes(int[][] matrix)
    {
        HashSet<int> zerod_rows = new HashSet<int>();
        HashSet<int> zerod_cols = new HashSet<int>();

        // iterate through each element
        for(int row=0; row<matrix.Length; row++)
        {
            for(int col=0; col<matrix[0].Length; col++)
            {
                // check if element is zero
                if(matrix[row][col]==0)
                {
                    // record row & col
                    zerod_rows.Add(row);
                    zerod_cols.Add(col);
                }
            }
        }
        // apply zero to appropriate cols
        foreach(var col in zerod_cols)
        {
            for(int row=0; row<matrix.Length; row++)
                matrix[row][col] = 0;
        }
        // apply zero to appropriate rows
        foreach(var row in zerod_rows)
        {
            for(int col=0; col<matrix[0].Length; col++)
                matrix[row][col] = 0;
        }

    }
}
```