# Distinct Subsequences

## Distinct subsequences equal to target
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

## All distinct subsequences

The solution processes the string **from right to left** and maintains two values:

- `distinctSubsequences` — the number of distinct non-empty subsequences in the already processed suffix.
- `countsOfSuffixesStartedAtLetter[c]` — the number of such subsequences that start with character `c`.

For the current character `c`, it is possible to form `distinctSubsequences + 1` subsequences: the character `c` itself, plus `c + subsequence` for every existing subsequence.

Some of these may already exist. Since every newly formed subsequence starts with `c`, duplicates can only come from subsequences that also start with `c`. Therefore:

```text
added = distinctSubsequences + 1 - count[c]
```

The value `added` is then added both to the total number of distinct subsequences and to the number of subsequences starting with `c`.

```C#
public int NumDistinct(string s)
{
	const int AlphabetSize = 26;
	const int Modulo = 1_000_000_007;

	int n = s.Length;
	if (n == 1) {
		return 1;
	}

	int distinctSubsequences = 0;
	var countsOfSubsequencesStartedAtLetter = new int[AlphabetSize];
	for (int i = n - 1; i >= 0; --i) {
		int letterCode = s[i] - 'a';
		int added = (distinctSubsequences + 1 + Modulo - countsOfSubsequencesStartedAtLetter[letterCode]) % Modulo;
		countsOfSubsequencesStartedAtLetter[letterCode] = (countsOfSubsequencesStartedAtLetter[letterCode] + added) % Modulo;
		distinctSubsequences = (distinctSubsequences + added) % Modulo;
	}
	return distinctSubsequences;
}
```

The classic DP solution usually processes the string **from left to right**. It effectively doubles the number of subsequences when adding a new character and subtracts duplicates caused by the previous occurrence of that character.

This solution is a symmetric variant:

```text
Classic:  left → right, tracks duplicates by previous occurrences
This one: right → left, groups subsequences by their first character
```

Time complexity: $O(n)$
Space complexity: $O(1)$

[[dynamic_programming|Dynamic programming]]
