[https://leetcode.com/problems/path-crossing[https://leetcode.com/problems/finding-3-digit-even-numbers](https://leetcode.com/problems/finding-3-digit-even-numbers)

# Intuition
My first thought was to use a sliding window to find all of the possible 3 digit groupings, then separately find each possible permutation of numbers within each grouping.

This flawed first thinking stemmed from one issue off the get-go: **I did NOT read the problem thorougly enough** and thus I thought each group of 3 numbers had to be next to each other (consecutive). However, I later realized you could group any 3 numbers regardless of proximity, which later gave rise to the `ReduceDigits()` method which allows the solution to run somewhat performately.

# Approach
1. Reduce digits to a maximum of 3 of each digit (3 0's, 3 1's, etc.)
2. Find all possible unique groupings of 3 digits from the pool
3. Check for leading zero
4. Check last digit is even
5. Return all even numbers that have passed all criteria

# Complexity
- Time complexity:
Essentially `O(n)` but it's a bit of a complicated calculation - note there is an aboslute maximum of n=30 (3*10)

- Space complexity:
Also should be linear, so `O(n)`

# Code

```
class TupleComparer : IEqualityComparer<Tuple<int,int,int>> {
    public bool Equals(Tuple<int,int,int> x, Tuple<int,int,int> y)
    {
        var x_ordered = new List<int>(){x.Item1, x.Item2, x.Item3};
        x_ordered.Sort();
        var y_ordered = new List<int>(){y.Item1, y.Item2, y.Item3};
        y_ordered.Sort();
        return x_ordered[0] == y_ordered[0] && x_ordered[1] == y_ordered[1] && x_ordered[2] == y_ordered[2];
    }

    public int GetHashCode(Tuple<int,int,int> source)
    {
        int hash = 0;
        hash = hash ^ source.Item1.GetHashCode();
        hash = hash ^ source.Item2.GetHashCode();
        hash = hash ^ source.Item3.GetHashCode();
        return hash;
    }
}
public class Solution {
    public void GetPermutations(Tuple<int,int,int> digits, ref HashSet<int> hashes)
    {
        if(digits.Item1 != 0)
        {
            if(digits.Item3 % 2 == 0)
                hashes.Add(digits.Item1 * 100 + digits.Item2 * 10 + digits.Item3);
            if(digits.Item2 % 2 == 0)
                hashes.Add(digits.Item1 * 100 + digits.Item3 * 10 + digits.Item2);
        }
        if(digits.Item2 != 0)
        {
            if(digits.Item3 % 2 == 0)
                hashes.Add(digits.Item2 * 100 + digits.Item1 * 10 + digits.Item3);
            if(digits.Item1 % 2 == 0)
                hashes.Add(digits.Item2 * 100 + digits.Item3 * 10 + digits.Item1);
        }
        if(digits.Item3 != 0)
        {
            if(digits.Item2 % 2 == 0)
                hashes.Add(digits.Item3 * 100 + digits.Item1 * 10 + digits.Item2);
            if(digits.Item1 % 2 == 0)
                hashes.Add(digits.Item3 * 100 + digits.Item2 * 10 + digits.Item1);
        }

    }
    public HashSet<Tuple<int,int,int>> GetGroups(int[] digits)
    {
        HashSet<Tuple<int,int,int>> hashes = new HashSet<Tuple<int,int,int>>(new TupleComparer());
        for(int i=0; i<digits.Length; i++)
        {
            var item1 = digits[i];
            for(int j=0; j<digits.Length; j++)
            {
                if(i==j)
                    continue;
                var item2 = digits[j];
                for(int k=0; k<digits.Length; k++)
                {
                    if(i==k || j==k)
                        continue;
                    var item3 = digits[k];
                    Tuple<int,int,int> group = new Tuple<int,int,int>(item1, item2, item3);
                    hashes.Add(group);
                }
            }
        }
        return hashes;
    }
    int[] ReduceDigits(int[] digits)
    {
        List<int> new_digits = new List<int>();
        for(int i=0; i<10; i++)
        {
            int count = digits.Count(d => d==i);
            for(int j=0; j<count && j<=3; j++)
            {
                new_digits.Add(i);
            }
        }
        return new_digits.ToArray();
    }
    public int[] FindEvenNumbers(int[] digits)
    {
        var new_digits = ReduceDigits(digits);
        var groups = GetGroups(new_digits);
        Console.WriteLine($"Found {groups.Count()} groups:");
        foreach(var group in groups)
            Console.WriteLine($"{group.Item1}{group.Item2}{group.Item3}");
        HashSet<int> evenNums = new HashSet<int>();
        foreach(var group in groups)
        {
            GetPermutations(group, ref evenNums);
        }
        var result = evenNums.ToList();
        result.Sort();
        return result.ToArray();
    }
}
```