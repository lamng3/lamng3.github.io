---
layout: post
category: leetcode
title: USACO Silver 2019 February - Painting The Barn
snippet: USACO Silver
tags: [algorithms, usaco, silver-hard]
katex: true
---

Give this problem a try [USACO Silver 2019 February - Painting The Barn](https://usaco.org/index.php?page=viewproblem2&cpid=919).

---

### Approach

This problem reminds me of [Difference Array](https://codeforces.com/blog/entry/78762) approach, where essentially we are trying to count how many times each point in the grid is visited by the rectangles. Given a 1D problem, it is easy to directly apply Difference Array technique. However, when looking at 2D grid like this, we need to expand the approach. A 2D Prefix Sum is the candidate in this case. 

A **naive** approach would be to iterate through each point of the rectangles and fill up. However, this approach would have the complexity to be **$O(N * WIDTH^2)$**, which will be TLE. 

A **better** approach would be to use Difference Array. For each of the rectangle, we iterate the rows and mark starting point as 1 and ending point as -1 (because we are considering points, not cells). An example of this would be the area of (1,1,5,5) is 16, not 25. Therefore, we can optimize our code to be **$O(N*WIDTH + WIDTH^2)$**. However, this will still be TLE as N is large.

The **best** approach involve using 2D Prefix Sum. Now the task becomes summing the sub-rectangles within the grid. Consider $f(i,j)$ to be the sum of all points $g(k,l)$ with $0 \leq k \leq i$ and $0 \leq l \leq j$. Therefore, we can calculate the value of $f(i,j)$ as $f(i,j) = f(i-1,j) + f(i,j-1) - f(i-1,j-1) + g(i,j)$. Note that we have 4 points as border points in the rectangle, so we will mark lower-left and upper-right as 1, and upper-left and lower-right as -1 to complete the prefix sum calculation. By doing this, we reduced the complexity to  **$O(N + WIDTH^2)$**, which will pass the test cases.

---

### Example Code

```java
import java.io.*;
import java.util.*;

public class Solution {
    public static void main(String[] args) throws Exception {
        FastScanner io = new FastScanner("paintbarn");
		// FastScanner io = new FastScanner();
		
		int N = io.nextInt();
		int K = io.nextInt();

		int WIDTH = 1000;

		int[][] dp = new int[WIDTH+1][WIDTH+1];

		// this is similar to difference array technique
		// but we are not considering cell (x2, y2)
		// as this is a point
		// e.g. (1,1,5,5) area is 16 not 25

		for (int i = 0; i < N; i++) {
			int x1 = io.nextInt(), y1 = io.nextInt();
			int x2 = io.nextInt(), y2 = io.nextInt();
			dp[x1][y1]++; // mark the start as 1
			dp[x2][y2]++; // mark the start as 1
			dp[x2][y1]--; // mark the end as -1
			dp[x1][y2]--; // mark the end as -1
		}

		int answer = 0;
		for (int i = 0; i <= WIDTH; i++) {
			for (int j = 0; j <= WIDTH; j++) {
				if (i > 0) dp[i][j] += dp[i-1][j];
				if (j > 0) dp[i][j] += dp[i][j-1];
				if (i > 0 && j > 0) dp[i][j] -= dp[i-1][j-1];
				if (dp[i][j] == K) answer++;
			}
		}

		io.println(answer);
		io.close();
    }

    /**
        RESERVED NUMBERS
    */
    public static int MOD = 1_000_000_007; // prime number

    /**
        DATA STRUCTURES
    */
    static class MultiSet {
        static TreeMap<Integer, Integer> multiset = new TreeMap<>();
        static void add(int x) {
            multiset.putIfAbsent(x, 0);
            multiset.put(x, multiset.get(x)+1);
        }
        static void remove(int x) {
            multiset.putIfAbsent(x, 0);
            multiset.put(x, multiset.get(x)-1);
            if (multiset.get(x) <= 0) multiset.remove(x);
        }
    }

    /**
        IO
    */
    static class FastScanner extends PrintWriter {
        private BufferedReader br;
        private StringTokenizer st;
		
		// standard input
        public FastScanner() { this(System.in, System.out); }
		public FastScanner(InputStream i, OutputStream o) {
            super(o);
			st = new StringTokenizer("");
            br = new BufferedReader(new InputStreamReader(i));
        }
		// USACO-style file input
        public FastScanner(String problemName) throws IOException {
            super(problemName + ".out");
			st = new StringTokenizer("");
            br = new BufferedReader(new FileReader(problemName + ".in"));
        }

        // returns null if no more input
        public String next() {
            try {
                while (st == null || !st.hasMoreTokens())
                    st = new StringTokenizer(br.readLine());
                return st.nextToken();
            } catch (Exception e) { }
            return null;
        }
        public int nextInt() { return Integer.parseInt(next()); }  
        public double nextDouble() { return Double.parseDouble(next()); }   
        public long nextLong() { return Long.parseLong(next()); }   
    }
}
```

#### Complexity Analysis

- Time Complexity: $O(N + WIDTH^2)$
- Space Complexity: $O(WIDTH^2)$

---

### Sign-off

Congratulations on making it this far! Best of luck in your future competitions!