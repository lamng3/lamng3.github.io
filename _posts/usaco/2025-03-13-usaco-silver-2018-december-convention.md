---
layout: post
category: leetcode
title: USACO Silver 2018 December - Convention
snippet: USACO Silver
tags: [algorithms, usaco, silver]
---

Give this problem a try [USACO Silver 2018 December - Convention](http://www.usaco.org/index.php?page=viewproblem2&cpid=858).

---

### Approach

First, the cows are sorted by their arrival times. The problem is then reformulated as determining the minimum waiting time x that allows all cows to be placed into M buses. Or in other words, what is the minimum number of busses required to allocate all cows with minimum waiting time x. Given that the waiting time function is monotonic, we can use binary search on x. For each candidate x, we verify whether it is feasible to assign the cows to M buses such that no cow's waiting time exceeds x.

---

### Example Code

```java
import java.io.*;
import java.util.*;

public class Solution {
	public static boolean check(long x, long[] cows, int N, int M, int C) {
		long first = cows[0];
		int used = 1;
		int cap = 0;

		// minimum buses needed to have maximum wait time = x
		for (int i = 0; i < N; i++) {
			if (cows[i] - first > x || cap >= C) {
				first = cows[i];
				used++;
				cap = 0;
			}
			cap++; // add cow to bus
		}

		return used <= M;
	}

	// find maximum waiting time
	public static long search(long[] cows, int N, int M, int C) {
		long max = 1_000_000_001;
		long pos = max;

		for (long x = max; x >= 1; x /= 2) {
			while (check(pos - x, cows, N, M, C)) {
				pos -= x;
			}
		}

		return pos;
	}

    public static void main(String[] args) throws Exception {
        FastScanner io = new FastScanner("convention"); // usaco submission file
		// FastScanner io = new FastScanner();

		int N = io.nextInt(), M = io.nextInt(), C = io.nextInt();
		long[] cows = new long[N];
		for (int i = 0; i < N; i++) { cows[i] = io.nextLong(); }
		
		Arrays.sort(cows);

		long answer = search(cows, N, M, C);
		io.println(answer);

		io.close();
    }

    /**
        SPECIAL NUMBERS
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
- Space Complexity: O(N)

---

### Sign-off

Congratulations on making it this far! Best of luck in your future competitions!