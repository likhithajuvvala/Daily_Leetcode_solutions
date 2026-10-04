# Daily_Leetcode_solutions
# 678. Valid Parenthesis String

**Difficulty:** Medium | **Topics:** String, Greedy, Dynamic Programming, Stack

[LeetCode Problem](https://leetcode.com/problems/valid-parenthesis-string/)

## Problem
Given a string `s` containing only `'('`, `')'` and `'*'`, return `true` if `s` is valid.

- Every `(` must have a matching `)`, and every `)` must have a matching `(`.
- `(` must come before its matching `)`.
- `*` can be treated as `(`, `)`, or an empty string.

**Examples**

| Input | Output |
|-------|--------|
| `"()"` | `true` |
| `"(*)"` | `true` |
| `"(*))"` | `true` |
| `"*("` | `false` |

## Approaches

### 1. Brute force / recursion
Try all 3 options for every `*`. Time: O(3^k). With memoization on `(index, openCount)` it becomes O(n²) time and O(n²) space.

### 2. Two-pass greedy
- Left to right, treat `*` as `(` to catch too many `)`.
- Right to left, treat `*` as `)` to catch too many `(`.

Time: O(n), Space: O(1).

### 3. Single pass with a min/max range (optimal)
Track the range of possible unclosed `(` counts.

| Char | min | max |
|------|-----|-----|
| `(` | +1 | +1 |
| `)` | -1 | -1 |
| `*` | -1 | +1 |

- If `max < 0`: too many `)`, return `false`.
- If `min < 0`: reset `min = 0`, since the count can't be negative.
- At the end: valid if `min == 0`.

**Why a range works:** If the possible counts are `{1, 2, 3}`, a `*` turns them into `{0, 1, 2, 3, 4}`. There are no gaps, so the whole set can be described by its smallest and largest values.

**Dry run: `"(*))"`**

| char | min | max |
|------|-----|-----|
| `(` | 1 | 1 |
| `*` | 0 | 2 |
| `)` | 0 | 1 |
| `)` | 0 | 0 |

`min == 0`, so the result is `true`.

**Why order matters: `"*("`** gives `min = 1` at the end, so it returns `false`. The `*` comes before the `(`, so it can't close it.

## Complexity
- **Time:** O(n)
- **Space:** O(1)

## Edge Cases
`""`, `"*"`, `")("`, `"(("`, `"(*)"`, `"*("`

## Key Takeaway
When a problem has many "wildcard" choices but you only care about an aggregate (here, the open count), track the range of possible values instead of the individual choices.
