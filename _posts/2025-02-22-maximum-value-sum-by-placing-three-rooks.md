---
layout: post
category: blog
title: Maximum Value Sum by Placing Three Rooks I & II
snippet: Biweekly Practice 137
tags: [algorithms, leetcode, hard]
---

Give this problem series a try [3256. Maximum Value Sum by Placing Three Rooks I](https://leetcode.com/problems/maximum-value-sum-by-placing-three-rooks-i/description/), [3257. Maximum Value Sum by Placing Three Rooks II](https://leetcode.com/problems/maximum-value-sum-by-placing-three-rooks-ii/description/).

---

### Problem Statement

You are given a `m x n` 2D array board representing a chessboard, where `board[i][j]` represents the value of the cell `(i, j)`.

Rooks in the same row or column attack each other. You need to place three rooks on the chessboard such that the rooks do not attack each other.

Return the maximum sum of the cell values on which the rooks are placed.

**Example 1:**

- **Input:** 
  `board = [[-3,1,1,1],[-3,1,-3,1],[-3,2,1,1]]`

- **Output:** 
  `4`

- **Explanation:** 
    We can place the rooks in the cells `(0, 2)`, `(1, 3)`, and `(2, 1)` for a sum of `1 + 1 + 2 = 4`.

**Example 2:**

- **Input:** 
  `board = [[1,2,3],[4,5,6],[7,8,9]]`

- **Output:** 
  `15`

- **Explanation:** 
    We can place the rooks in the cells `(0, 0)`, `(1, 1)`, and `(2, 2)` for a sum of `1 + 5 + 9 = 15`.

**Example 3:**

- **Input:** 
  `board = [[1,1,1],[1,1,1],[1,1,1]]`

- **Output:** 
  `3`

- **Explanation:** 
    We can place the rooks in the cells `(0, 2)`, `(1, 1)`, and `(2, 0)` for a sum of `1 + 1 + 1 = 3`.

#### Constraints

**3256:**
- `3 <= m == board.length <= 100`
- `3 <= n == board[i].length <= 100`
- `-1e9 <= board[i][j] <= 1e9`

**3257:**
- `3 <= m == board.length <= 500`
- `3 <= n == board[i].length <= 500`
- `-1e9 <= board[i][j] <= 1e9`

---

### Prerequisite Knowledge

- Prefix / Suffix Sum

---

### Example Code
```java
class Solution {
    private int[][] findTop3(int[] arr) {
        int n = arr.length;
        int[][] out = new int[3][2];
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> b[0] - a[0]);
        for (int i = 0; i < n; i++) pq.add(new int[] {arr[i], i});
        for (int i = 0; i < 3; i++) out[i] = pq.poll();
        return out;
    }
    
    public long maximumValueSum(int[][] board) {
        int m = board.length;
        int n = board[0].length;

        int[][] maxPref = new int[m][n];
        int[][] maxSuff = new int[m][n];

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                maxPref[i][j] = Integer.MIN_VALUE;
                maxSuff[i][j] = Integer.MIN_VALUE;
            }
        }

        maxPref[0] = board[0];
        maxSuff[m-1] = board[m-1];

        for (int j = 0; j < n; j++) {
            for (int i = 1; i < m; i++) maxPref[i][j] = Math.max(maxPref[i-1][j], board[i][j]);
            for (int i = m-2; i >= 0; i--) maxSuff[i][j] = Math.max(maxSuff[i+1][j], board[i][j]);
        }

        long answer = Long.MIN_VALUE;

        for (int i = 1; i < m-1; i++) {
            int[][] A = findTop3(maxPref[i-1]);
            int[][] B = findTop3(board[i]);
            int[][] C = findTop3(maxSuff[i+1]);

            for (int[] a : A) {
                for (int[] b : B) {
                    for (int[] c : C) {
                        if (a[1] != b[1] && b[1] != c[1] && c[1] != a[1]) {
                            long sum = 1L * a[0] + 1L * b[0] + 1L * c[0];
                            answer = Math.max(answer, sum);
                        }
                    }
                }
            }
        }

        return answer;
    }
}
```

#### Complexity Analysis
- Time Complexity: O(m*n)
- Space Complexity: O(m*n)

---

### (Special) Explanation


---

### Sign-off

Congratulations on making it this far! Best of luck in your future competitions!