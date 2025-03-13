---
layout: post
category: leetcode
title: USACO Silver 2016 January - Angry Cows
snippet: USACO Silver
tags: [algorithms, usaco, silver]
---

Give this problem a try [USACO Silver 2016 January - Angry Cows](http://usaco.org/index.php?page=viewproblem2&cpid=594).

---

### Approach

We perform binary search on the power R in the range [ 1, x[N]-x[1] ] to determine the minimal R needed such that the number of cows used does not exceed the allowed limit. For each candidate R, we adopt a greedy strategy: always target the leftmost undetonated hay bale, launch a cow so that its explosion at x[i] + R covers the interval [ x[i], x[i]+2R ], and then skip over all hay bales within this covered range. The task now becomes finding the minimum number of cows with power R needed to detonate all hay bales.

---

### Example Code

```java
import java.io.*;
import java.util.*;

public class Solution {
	public static boolean check(long R, int N, int K, long[] hays) {
		long end = hays[0] + 2 * R;
		int used = 1;
		for (int i = 1; i < N; i++) {
			if (hays[i] <= end) continue;
			end = hays[i] + 2 * R;
			used++;
		}
		return used <= K;
	}

	public static long search(long upper, int N, int K, long[] hays) {
		long pos = upper;
		for (long R = upper; R >= 1; R /= 2) {
			while (check(pos - R, N, K, hays)) {
				pos -= R;
			}
		}
		return pos;
	}

    public static void main(String[] args) throws Exception {
        FastScanner io = new FastScanner("angry");
		// FastScanner io = new FastScanner();

		int N = io.nextInt(), K = io.nextInt();
		long[] hays = new long[N];
		for (int i = 0; i < N; i++) hays[i] = io.nextLong();
		Arrays.sort(hays);

		long upper = hays[N-1] - hays[0];
		long answer = search(upper, N, K, hays);
		io.println(answer);

		io.close();
    }

    /**
        RESERVED NUMBERS
    */
    public static int MOD = 1_000_000_007;

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

- Time Complexity: O(NlogN)
- Space Complexity: O(NlogN)

---

### Sign-off

Congratulations on making it this far! Best of luck in your future competitions!