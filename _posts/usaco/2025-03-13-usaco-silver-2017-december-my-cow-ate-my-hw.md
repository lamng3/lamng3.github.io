---
layout: post
category: leetcode
title: USACO Silver 2017 December - My Cow Ate My Homework
snippet: USACO Silver
tags: [algorithms, usaco, silver]
katex: true
---

Give this problem a try [USACO Silver 2017 December - My Cow Ate My Homework](https://usaco.org/index.php?page=viewproblem2&cpid=762).

---

### Approach

---

### Example Code

```java
import java.io.*;
import java.util.*;

public class Solution {
    public static void main(String[] args) throws Exception {
        FastScanner io = new FastScanner("homework");
		// FastScanner io = new FastScanner();

		int N = io.nextInt();
		int[] hw = new int[N];
		for (int i = 0; i < N; i++) hw[i] = io.nextInt();

		// prefix sum of hw scores
		int[] pref = new int[N];
		pref[0] = hw[0];
		for (int i = 1; i < N; i++) pref[i] = pref[i-1] + hw[i]; 

		// suffix storing minimum value up to index ith from end
		int[] minSuff = new int[N];
		minSuff[N-1] = hw[N-1];
		for (int i = N-2; i >= 0; i--) minSuff[i] = Math.min(minSuff[i+1], hw[i]);

		Map<Double, List<Integer>> Ks = new HashMap<>();
		double maxAverageScore = 0;

		for (int K = 1; K <= N-2; K++) {
			int remaining = pref[N-1] - pref[K-1]; // sum K to N-1
			int minHw = minSuff[K]; // minHW up to K
			int score = remaining - minHw;
			double average = (double) score / (N - (K+1)); // cast score to double first
			maxAverageScore = Math.max(maxAverageScore, average);
			Ks.putIfAbsent(average, new ArrayList<>());
			Ks.get(average).add(K);
		}

		List<Integer> answer = Ks.get(maxAverageScore);
		for (int i = 0; i < answer.size(); i++) {
			io.println(answer.get(i));
		}

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