---
layout: post
category: leetcode
title: USACO Silver 2015 December - High Card Wins
snippet: USACO Silver
tags: [algorithms, usaco, silver, greedy]
katex: true
---

Give this problem a try [USACO Silver 2015 December - High Card Wins](https://usaco.org/index.php?page=viewproblem2&cpid=571).

---

### Approach



---

### Example Code

```java
import java.io.*;
import java.util.*;

public class Solution {
    public static void main(String[] args) throws Exception {
        FastScanner io = new FastScanner("highcard");
		// FastScanner io = new FastScanner();

		int N = io.nextInt();
		int[] taken = new int[2*N+1];

		int[] elsie = new int[N];
		for (int i = 0; i < N; i++) {
			int card = io.nextInt();
			taken[card] = 1;
			elsie[i] = card;
		}
		Arrays.sort(elsie);

		int[] bessie = new int[N];
		int idx = 0;
		for (int i = 1; i < 2*N+1; i++) {
			if (taken[i] == 0) bessie[idx++] = i;
		}

		int points = 0;

		int eptr = 0;
		int bptr = 0;
		while (eptr < N && bptr < N) {
			if (bessie[bptr] < elsie[eptr]) {
				bptr++;
				continue;
			}

			points++;
			bptr++;
			eptr++;
		}

		io.println(points);

		io.close();
    }

    /**
        RESERVED NUMBERS
    */
    public static int MOD = 1_000_000_007; // prime number
	public static int INF = 1_000_000_007; // infinity number

    /**
        DATA STRUCTURES
    */
    static class MultiSet {
        static TreeMap<Integer, Integer> multiset;
		public MultiSet() { multiset = new TreeMap<>(); }
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

- Time Complexity: 
- Space Complexity: 

---

### Sign-off

Congratulations on making it this far! Best of luck in your future competitions!