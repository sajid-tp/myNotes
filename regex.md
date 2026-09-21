

### A match succeeds when two things line up together:

- ^ — the match starts at index 0
- $ — the match, after consuming characters, lands exactly at the end of the string (no characters left unaccounted for)

### +, *, ?, and {} are all quantifiers: they control how many times the thing immediately before them must repeat.

The full set
Quantifier	Meaning	Example
+	1 or more	a+ → matches "a", "aa", "aaa", but not ""
*	0 or more	a* → matches "a", "aa", and also "" (empty is fine)
?	0 or 1 (optional)	a? → matches "" or "a", nothing more
{n}	exactly n	a{3} → matches "aaa" only, not "aa" or "aaaa"
{n,}	n or more (no upper limit)	a{2,} → matches "aa", "aaa", "aaaa", ...
{n,m}	between n and m, inclusive	a{2,4} → matches "aa", "aaa", "aaaa" — not "a" or "aaaaa"
