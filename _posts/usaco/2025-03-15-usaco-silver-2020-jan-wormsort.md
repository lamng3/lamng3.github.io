---
layout: post
category: leetcode
title: USACO Silver 2020 January - Wormhole Sort
snippet: USACO Silver
tags: [algorithms, usaco, silver]
katex: true
---

Give this problem a try [USACO Silver 2020 January - Wormhole Sort](https://usaco.org/index.php?page=viewproblem2&cpid=992).

---

### Approach



---

### Example Code

```java
import java.io.*;
import java.util.*;

public class Solution {
	static class DSU {
		int[] parent;
		int[] sz;

		public DSU(int N) {
			parent = new int[N];
			sz = new int[N];
			for (int i = 0; i < N; i++) {
				parent[i] = i;
				sz[i] = 1;
			}
		}

		public int find(int v) {
			if (parent[v] == v) return parent[v];
			return parent[v] = find(parent[v]);
		}

		public void union(int a, int b) {
			a = find(a);
			b = find(b);
			if (a != b) {
				if (sz[a] <= sz[b]) {
					int tmp = a;
					a = b;
					b = tmp;
				}
				parent[b] = a;
				sz[a] += sz[b];
			}
		}
	}

	public static boolean check(int x, int max, int[] cows, int[][] wormholes) {
		int N = cows.length;
		DSU dsu = new DSU(N);
		for (int[] wh : wormholes) {
			if (x <= wh[2]) dsu.union(wh[0], wh[1]);
		}
		boolean ok = true;
		for (int i = 0; i < N; i++) {
			int pcow = dsu.find(cows[i]);
			int pi = dsu.find(i);
			if (pcow != pi) {
				ok = false;
				break;
			}
		}
		return ok;
	}

	public static int search(int max, int[] cows, int[][] wormholes) {
		int pos = 0;
		for (int x = max; x >= 1; x /= 2) {
			while (check(pos+x, max, cows, wormholes)) {
				pos += x;
			}
		}
		return pos;
	}

    public static void main(String[] args) throws Exception {
        FastScanner io = new FastScanner("wormsort");
		// FastScanner io = new FastScanner();
		
		int N = io.nextInt(), M = io.nextInt();

		int[] cows = new int[N];
		boolean sorted = true;
		for (int i = 0; i < N; i++) {
			cows[i] = io.nextInt()-1;
			if (cows[i] != i) sorted = false;
		}
		if (sorted) {
			io.println(-1); 
			io.close(); 
			return;
		}

		int[][] wormholes = new int[M][3];
		int maxWidth = 0;
		for (int i = 0; i < M; i++) {
			wormholes[i] = new int[]{io.nextInt()-1, io.nextInt()-1, io.nextInt()};
			maxWidth = Math.max(maxWidth, wormholes[i][2]);
		}
		Arrays.sort(wormholes, (a, b) -> a[2] - b[2]);

		int answer = search(maxWidth+1, cows, wormholes);
		io.println(answer);

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