---
layout: post
category: blog
title: 3458. Select K Disjoint Special Substrings
snippet: Weekly Contest 437
tags: [algorithms, leetcode, medium]
---

Give this problem a try [3458. Select K Disjoint Special Substrings](https://leetcode.com/problems/select-k-disjoint-special-substrings/description/)

---

### Problem Statement

Given a string `s` of length `n` and an integer `k`, determine whether it is possible to select `k` disjoint special substrings.

A **special substring** is a substring where:
- **Any character present inside the substring should not appear outside it in the string.**
- **The substring is not the entire string `s`.**

**Note:** All `k` substrings must be disjoint, meaning they cannot overlap.

**Return:**  
Return `true` if it is possible to select `k` such disjoint special substrings; otherwise, return `false`.

#### Examples

**Example 1:**

- **Input:**  
  `s = "abcdbaefab", k = 2`
- **Output:**  
  `true`
- **Explanation:**  
  We can select two disjoint special substrings: `"cd"` and `"ef"`.  
  - `"cd"` contains the characters `'c'` and `'d'`, which do not appear elsewhere in `s`.  
  - `"ef"` contains the characters `'e'` and `'f'`, which do not appear elsewhere in `s`.

**Example 2:**

- **Input:**  
  `s = "cdefdc", k = 3`
- **Output:**  
  `false`
- **Explanation:**  
  There can be at most 2 disjoint special substrings: `"e"` and `"f"`.  
  Since `k = 3`, the output is `false`.

**Example 3:**

- **Input:**  
  `s = "abeabe", k = 0`
- **Output:**  
  `true`

#### Constraints

- `2 <= n == s.length <= 5 * 10^4`
- `0 <= k <= 26`
- `s` consists only of lowercase English letters.

---

### Prerequisite Knowledge

- Two Pointers
- Greedy
- Sorting

---

### Example Code

```java
class Solution {
    private int[][] preprocess(String s) {
        int n = s.length();
        
        int[][] itv = new int[26][2];
        for (int i = 0; i < 26; i++) itv[i] = new int[] {n,-1};

        for (int i = 0; i < n; i++) {
            int charCode = s.charAt(i) - 'a';
            itv[charCode][0] = Math.min(itv[charCode][0], i);
            itv[charCode][1] = Math.max(itv[charCode][1], i);
        }

        return itv;
    }

    private boolean isOverlap(int[] a, int[] b) {
        return !(a[1] < b[0] || b[1] < a[0]);
    }

    public boolean maxSubstringLength(String s, int k) {
        int n = s.length();
        int[][] itv = preprocess(s);

        // Maximally extend each character's intervals
        List<int[]> eligible = new ArrayList<>();
        for (int c = 0; c < 26; c++) {
            int[] boundary = itv[c];
            if (boundary[1] == -1) continue;

            // Extend left and right of the interval boundaries
            // Left and right pointers run in opposite directions
            int lptr = boundary[0], rptr = boundary[0];
            while (lptr > boundary[0] || rptr < boundary[1]) {
                if (boundary[0] == 0 && boundary[1] == n-1) break;
                if (rptr < boundary[1]) {
                    rptr++;
                    int i = s.charAt(rptr) - 'a';
                    boundary[0] = Math.min(boundary[0], itv[i][0]);
                    boundary[1] = Math.max(boundary[1], itv[i][1]);
                    continue;
                }
                lptr--;
                int i = s.charAt(lptr) - 'a';
                boundary[0] = Math.min(boundary[0], itv[i][0]);
                boundary[1] = Math.max(boundary[1], itv[i][1]);
            }

            if (boundary[0] == 0 && boundary[1] == n-1) continue;
            eligible.add(boundary);
        }

        // Sort by right-end to minimize chosen range per choice
        Collections.sort(eligible, (a, b) -> a[1] - b[1]);

        // Greedy:
        // Select eligible from left to right
        // Compare with previously selected disjoint intervals
        List<int[]> disjoint = new ArrayList<>();
        for (int[] r : eligible) {
            boolean ok = true;
            for (int[] r2 : disjoint) {
                if (isOverlap(r, r2)) {
                    ok = false;
                    break;
                }
            }
            if (ok) disjoint.add(r);
        }
        // [[2 5], [1 6], [6 6]] -> sorted right-end: prioritize [2 5] and [6 6] -> select 2 (more optimal)
        // [[1 6], [2 5], [6 6]] -> sorted left-end: prioritize [1 6] -> select 1

        return disjoint.size() >= k;
    }
}
```

#### Complexity Analysis
- Time Complexity: O(nlogn)
- Space Complexity: O(n)

---

### Sign-off

Congratulations on making it this far. I believe this problem is **medium-hard**, as it demands an elegant blend of straightforward techniques and keen observation. Best of luck in the future competitions!