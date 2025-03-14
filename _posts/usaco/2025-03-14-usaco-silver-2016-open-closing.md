---
layout: post
category: leetcode
title: USACO Silver 2016 Open - Closing The Farm
snippet: USACO Silver
tags: [algorithms, usaco, silver]
katex: true
---

Give this problem a try [USACO Silver 2016 Open - Closing The Farm](https://usaco.org/index.php?page=viewproblem2&cpid=644).

---

### Approach

This problem basically checks if the farm is still "fully connected" after closing the barns. I am using DSU to check the fully connected property, but this can also be done with simple DFS. 

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
				if (sz[a] < sz[b]) {
					int tmp = a;
					a = b;
					b = tmp;
				}
				parent[b] = a;
				sz[a] += sz[b];
			}
		}
	}
	
	// O(N)
	public static boolean connected(int N, Set<Integer> closed, int[][] edge) {
		DSU dsu = new DSU(N);
		for (int[] e : edge) {
			if (closed.contains(e[0]) || closed.contains(e[1])) continue;
			dsu.union(e[0], e[1]);
		}

		Set<Integer> parents = new HashSet<>();
		for (int i = 0; i < N; i++) {
			if (closed.contains(i)) continue;
			int p = dsu.find(i);
			parents.add(p);
		}
		return parents.size() == 1;
	}

    public static void main(String[] args) throws Exception {
        FastScanner io = new FastScanner("closing");
		// FastScanner io = new FastScanner();
		
		int N = io.nextInt(), M = io.nextInt();
		
		int[][] edge = new int[M][2];
		for (int i = 0; i < M; i++) {
			edge[i] = new int[]{io.nextInt()-1, io.nextInt()-1};
		}

		Set<Integer> closed = new HashSet<>();

		for (int i = 0; i < N; i++) {
			int barn = io.nextInt()-1;
			boolean isConnected = connected(N, closed, edge);
			io.println(isConnected ? "YES" : "NO");
			closed.add(barn);
		}

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

- Time Complexity: $O(N^2 + N \times M)$
- Space Complexity: $O(N + M)$

---

### Sign-off

Congratulations on making it this far! Best of luck in your future competitions!