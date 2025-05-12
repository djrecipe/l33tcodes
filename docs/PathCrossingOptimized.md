[https://leetcode.com/problems/path-crossing](https://leetcode.com/problems/path-crossing)

# Intuition
Implement an unoptimized solution by just recording each traveled node.

**NOTE: after a ton of analysis of the proposed solutions on l33tcode, I can confirm that this solution is actually quite well optimized - there does not seem to be a trick capable of detecting a crossing without recording each and every position**

# Approach
- Record each traveled node by combining X and Y into a single number and store in a HashSet
- Iterate through all characters in the array until a previously traveled node is encountered, then exit.

# Complexity
Time complexity:
`O(n)` (iterate through each position once)

Space complexity:
`O(n)` (store each position as hash)

# Code

```
public class Solution
{
    class PointComparer : IEqualityComparer<System.Drawing.Point> {
        public bool Equals(System.Drawing.Point x, System.Drawing.Point y) {
            return x.X == y.X && x.Y == y.Y;
        }

        public int GetHashCode(System.Drawing.Point obj) {
            return (obj.Y << 16) ^ obj.X;
        }
    }
    private HashSet<System.Drawing.Point> hashes = new HashSet<System.Drawing.Point>(new PointComparer());
    private System.Drawing.Point currentPos = new System.Drawing.Point(0,0);
    private bool OccupyCurrentPos()
    {
        //Console.WriteLine($"Calculated hash {hash} for position {currentPos.X}, {currentPos.Y}");
        if(hashes.Contains(currentPos))
            return true;
        hashes.Add(currentPos);
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