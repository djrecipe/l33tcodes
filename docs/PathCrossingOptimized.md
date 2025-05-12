[https://leetcode.com/problems/path-crossing](https://leetcode.com/problems/path-crossing)

# Intuition
Implement an Unoptimized Solution by just recording each traveled node.

# Approach
- Record each traveled node by combining X and Y into a single number and store in a HashSet
- Iterate through all characters in the array until a previously traveled node is encountered, then exit.

# Complexity
Time complexity:
$$O(n)$$ (iterate through each position once)

Space complexity:
$$O(n)$$ (store each position as hash)

# Code

```
public static class Extensions 
{
    public static int GetHash(this System.Drawing.Point point)
    {
        return ((point.X<<16) | ((point.Y) & 0xffff));
    }
}
public class Solution
{
    private HashSet<int> hashes = new HashSet<int>();
    private System.Drawing.Point currentPos = new System.Drawing.Point(0,0);
    private bool OccupyCurrentPos()
    {
        var hash = currentPos.GetHash();
        //Console.WriteLine($"Calculated hash {hash} for position {currentPos.X}, {currentPos.Y}");
        if(hashes.Contains(hash))
            return true;
        hashes.Add(hash);
        return false;
    }
    public bool IsPathCrossing(string path)
    {
        OccupyCurrentPos();
        foreach(char c in path.ToLower())
        {
            switch(c)
            {
                case 'n':
                    currentPos.Y++;
                    break;
                case 's':
                    currentPos.Y--;
                    break;
                case 'e':
                    currentPos.X++;
                    break;
                case 'w':
                    currentPos.X--;
                    break;
            }
            if(OccupyCurrentPos())
            {
                //Console.WriteLine($"Crossed at {currentPos.X},{currentPos.Y}");
                return true;
            }
        }
        return false;
    }
}
```