[https://leetcode.com/problems/total-characters-in-string-after-transformations-i](https://leetcode.com/problems/total-characters-in-string-after-transformations-i)

# Intuition

My first intuition was that I could determine the output of a given letter after `t` iterations, independent of the other letters. (I was wrong)

# Approach

My first approach was to try and use a combination of modulus and dividing the iteration count `t` by 26 to determine the end result of each letter. The goal here was to NOT iterate through `t` and aim for a fixed complexity rather than `O(n)`. In the end I found the branching logic too difficult for high iterations of `t`. With high enough iterations, a single letter would spawn many other letters and I could not capture this logic with a math equation.

Secondly I tried counting all the letters and putting them into buckets., then performing a similar pass as previously discussed. This also did not work.

Finally, after peeking at the answer, I resigned to iterating through `t` for `O(n)` complexity. I broke-down and re-wrote the answer in order to fully understand it.

In the end I still wonder if there is a calculation which could capture the end state after `t` transformations of even a single letter, without having to "perform the simulation" by iterating over `t`.

# Complexity

- Time complexity:

`O(n)`

- Space complexity:

Whatever 26 letters take up + the supporting code. Fixed cost.

# Code

```
public class Solution
{
    private const int MOD = 1000000007;

    public ulong[] CountChars(string s)
    {
        ulong[] char_counts = new ulong[26];
        foreach (char ch in s)
            char_counts[ch - 'a']++;
        return char_counts;    
    }

    public int LengthAfterTransformations(string s, int t)
    {
        var char_counts = CountChars(s);

        /*
        deciphered from https://leetcode.com/problems/total-characters-in-string-after-transformations-i/?envType=daily-question&envId=2025-05-13
        this solution cleverly defines letter counts based on where they could COME FROM, rather than what each letter is CONVERTED TO
        ...'a' can come from 'z'
        ...'b' can come from 'z' or 'a'
        ...others chars can come from their previous char

        this allows for a very clear formulaic approach
        -----------
        however, it is not possible to determine all yields of a given letter and number of steps.
        ...for example, if provided the only input string of letter 'a' but a long number of steps 't',
        ...it's still difficult to determine the result
        ... therefore, it IS actually necessary to step through each iteration (via 't')

        (oh and dont forget to apply modulator to reduce number size)
        */
        for (ulong i = 0; i < (ulong)t; ++i)
        {
            ulong[] next_counts = new ulong[26];
            // 'a' only comes from 'z'
            next_counts[0] = char_counts[25];

            // 'b' comes from 'a' and 'z'
            next_counts[1] = (char_counts[25] + char_counts[0]) % MOD;

            // remaining letters come from their previous
            for (ulong j = 2; j < 26; ++j)
                next_counts[j] = char_counts[j - 1] % MOD;

            char_counts = next_counts;
        }

        return (int)(char_counts.Aggregate((x,y) =>(x+y) % MOD));
    }
}
```