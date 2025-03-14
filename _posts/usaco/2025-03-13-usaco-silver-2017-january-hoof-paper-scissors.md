---
layout: post
category: leetcode
title: USACO Silver 2017 January - Hoof, Paper, Scissors
snippet: USACO Silver
tags: [algorithms, usaco, silver]
katex: true
---

Give this problem a try [USACO Silver 2017 January - Hoof, Paper, Scissors](https://usaco.org/index.php?page=viewproblem2&cpid=691).

---

### Approach

---

### Example Code

```java
import java.io.*;
import java.util.*;

public class Solution {
	public static int gesture(char c) {
		if (c == 'H') return 0;
		if (c == 'P') return 1;
		if (c == 'S') return 2;
		return -1;
	}

	public static int maxArray(int[] p) {
		return Math.max(p[0], Math.max(p[1], p[2]));
	}

    public static void main(String[] args) throws Exception {
        FastScanner io = new FastScanner("hps");
		// FastScanner io = new FastScanner();
		int N = io.nextInt();

		int[] gs = new int[N];
		for (int i = 0; i < N; i++) {
			char c = io.next().toCharArray()[0];
			int g = gesture(c);
			gs[i] = g;
		}

		// H = 0, P = 1, S = 2
		int[][] pref = new int[N][3];

		pref[0] = new int[]{0,0,0};
		pref[0][gs[0]]++;

		for (int i = 1; i < N; i++) {
			int[] p = pref[i-1];
			pref[i] = new int[]{p[0], p[1], p[2]};
			pref[i][gs[i]]++;
		}

		int[][] suff = new int[N][3];
		suff[N-1] = new int[]{0,0,0};
		suff[N-1][gs[N-1]]++;

		for (int i = N-2; i >= 0; i--) {
			int[] s = suff[i+1];
			suff[i] = new int[]{s[0], s[1], s[2]};
			suff[i][gs[i]]++;
		}
		
		int answer = 0;

		for (int i = 0; i < N-1; i++) { // switch at i
			int maxPref = maxArray(pref[i]);
			int maxSuff = maxArray(suff[i+1]);
			answer = Math.max(maxPref + maxSuff, answer);
		}
		answer = Math.max(maxArray(pref[N-1]), answer); // not switch

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

- Time Complexity: O(N)
- Space Complexity: O(N)

---

### Sign-off

Congratulations on making it this far! Best of luck in your future competitions!