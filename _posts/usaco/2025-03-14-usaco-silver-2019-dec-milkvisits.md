---
layout: post
category: leetcode
title: USACO Silver 2019 December - Milk Visits
snippet: USACO Silver
tags: [algorithms, usaco, silver]
katex: true
---

Give this problem a try [USACO Silver 2019 December - Milk Visits](https://usaco.org/index.php?page=viewproblem2&cpid=968).

---

### Approach

This problem requires finding connected components among the farms. The trick is that we need to find connected components by breed. Essentially, any given farms are initialized as a connected components. When an edge of 2 farms with cows of same breed is added, we union the two connect components. Given 2 connected components A (start) and B (end), we consider 2 cases:

- A and B belong to **same** component: This means that all farms in the path between A and B has cow of the same breed. If Farmer John's friend preferred that breed, output 1; otherwise 0.

- A and B belong to **different** component: This means that there will exist a farm of different breed when travel from A to B. This is proven by contradiction. Farmer's John connected his farms in tree-form. If between A and B all farms are of same breed, then A and B must belong to the same component, which contradicts. With this, Farmer John's friend will always be satisfied.

The technique used in this problem is [Disjoint Set Union (DSU)](https://cp-algorithms.com/data_structures/disjoint_set_union.html) to quick check connected components.

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

    public static void main(String[] args) throws Exception {
        FastScanner io = new FastScanner("milkvisits");
		// FastScanner io = new FastScanner();
		
		int N = io.nextInt(), M = io.nextInt();
		char[] breed = io.next().toCharArray();

		DSU dsu = new DSU(N);
		for (int i = 0; i < N-1; i++) {
			int X = io.nextInt() - 1, Y = io.nextInt() - 1;
			if (breed[X] == breed[Y]) dsu.union(X, Y);
		}

		StringBuilder sb = new StringBuilder();
		for (int i = 0; i < M; i++) {
			int A = io.nextInt() - 1, B = io.nextInt() - 1;
			char C = io.next().toCharArray()[0];
			A = dsu.find(A);
			B = dsu.find(B);
			if (A == B) sb.append(C == breed[A] ? 1 : 0);
			else sb.append(1);
		}

		io.println(sb.toString());

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

- Time Complexity: $O(N \times \alpha(N) + M \times \alpha(N))$ 
    - Where $\alpha(N)$ is the inverse Ackermann function, which grows very slowly. $\alpha(N) \leq 4$ for $N < 10^{600}$
    - Worst case scenario of DSU would be $O(logN)$
- Space Complexity: $O(N)$

---

### Sign-off

Congratulations on making it this far! Best of luck in your future competitions!