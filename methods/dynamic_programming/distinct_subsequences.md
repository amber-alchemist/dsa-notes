# Distinct Subsequences
The algorithm uses dynamic programming to count how many times `t` appears as a subsequence of `s`.
Let `dp[j]` be the number of ways to form the first `j` characters of `t` using the processed prefix of `s`. Initialize `dp[0] = 1`, since the empty string is always a subsequence. For each character in `s`, iterate through `t` from right to left. If the characters match, update:

$dp[j + 1] = dp[j + 1] + dp[j]$

The right-to-left iteration prevents the same character of `s` from being used more than once in a single step.

```C#
public int NumDistinct(string s, string t)
{
	int n = s.Length, m = t.Length;
	if (m > n) {
		return 0;
	}

	var dp = new int[m + 1];
	dp[0] = 1;
	for (int i = 0; i < n; ++i) {
		for (int j = m - 1; j >= 0; --j) {
			if (s[i] == t[j]) {
				dp[j + 1] += dp[j];
			}
		}
	}
	return dp[m];
}
```

Time complexity: $O(n * m)$
Space complexity: $O(m)$

Related problem: [LeetCode 115 — Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences).

[[dynamic_programming|Dynamic programming]]
