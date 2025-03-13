---
layout: post
category: leetcode
title: USACO Silver 2017 January - Cow Dance Show
snippet: USACO Silver
tags: [algorithms, usaco, silver, binary-search-value]
---

Give this problem a try [USACO Silver 2017 January - Cow Dance Show](https://usaco.org/index.php?page=viewproblem2&cpid=690).

---

### Approach

The intuition is to perform binary search on the value of K. The maximum value of K is 10,000 as $1 \leq K \leq N$. For each of the K, we need to check if the time needed for all cows to complete their dance (in other words, for the show to end) exceeds $T_max$. This is done by maintaining a Priority Queue for storing the time of the cows. Note that we add buffer time for each of the arriving cow, to account for the difference when we remove the previously finished cows. 

---

### Example Code

```java
import java.io.*;
import java.util.*;

public class Solution {
	public static boolean check(int k, int N, int Tmax, int[] d) {
		if (k <= 0) return false;
		
		PriorityQueue<Integer> cows = new PriorityQueue<>();
		int T = 0;

		for (int i = 0; i < N; i++) {
			if (cows.size() == k) {
				int cow = cows.poll();
				T = Math.max(T, cow);
			}
			cows.add(d[i] + T);
		}

		while (!cows.isEmpty()) {
			int cow = cows.poll();
			T = Math.max(T, cow);
		}

		return T <= Tmax;
	}

	public static int search(int N, int Tmax, int[] d) {
		int max = 10000;
		int k = max;
		for (int x = max; x >= 1; x/=2) {
			while (check(k-x, N, Tmax, d)) {
				k -= x;
			}
		}
		return k;
	}

    public static void main(String[] args) throws Exception {
        FastScanner io = new FastScanner("cowdance");
		// FastScanner io = new FastScanner();

		int N = io.nextInt(), Tmax = io.nextInt();
		int[] d = new int[N];
		for (int i = 0; i < N; i++) d[i] = io.nextInt();

		int answer = search(N, Tmax, d);
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

- Time Complexity: O(N*log(N)*log(N))
- Space Complexity: O(N)

---

### Sign-off

Congratulations on making it this far! Best of luck in your future competitions!