---
layout: post
category: leetcode
title: USACO Silver 2019 February - The Great Revegetation
snippet: USACO Silver
tags: [algorithms, usaco, silver]
katex: true
---

Give this problem a try [USACO Silver 2019 February - The Great Revegetation](https://usaco.org/index.php?page=viewproblem2&cpid=920).

---

### Approach



---

### Example Code

```java
import java.io.*;
import java.util.*;

public class Solution {
	public static int comp;
	public static int[] color;
	public static boolean impossible;
	public static ArrayList<Integer>[] same;
	public static ArrayList<Integer>[] diff;

	public static void visit(int x, int c) {
		if (impossible) return;
		color[x] = c;
		for (int next : same[x]) {
			if (color[next] == 3 - c) { impossible = true; }
			if (color[next] == 0) { visit(next, c); }
		}
		for (int next : diff[x]) {
			if (color[next] == c) { impossible = true; }
			if (color[next] == 0) { visit(next, 3-c); }
		}
	}

    public static void main(String[] args) throws Exception {
        FastScanner io = new FastScanner("revegetate");
		// FastScanner io = new FastScanner();
		
		int N = io.nextInt(), M = io.nextInt();

		impossible = false;
		comp = 0;
		same = new ArrayList[N+1];
		diff = new ArrayList[N+1];

		for (int i = 0; i <= N; i++) {
			same[i] = new ArrayList<>();
			diff[i] = new ArrayList<>();
		}

		for (int i = 0; i < M; i++) {
			char c = io.next().toCharArray()[0];
			int x = io.nextInt(), y = io.nextInt();
			if (c == 'S') {
				same[x].add(y);
				same[y].add(x);
			}
			if (c == 'D') {
				diff[x].add(y);
				diff[y].add(x);
			}
		}

		color = new int[N+1];
		for (int i = 1; i <= N; i++) {
			if (color[i] == 0) {
				visit(i, 1);
				comp++;
			}
		}

		if (impossible) { io.println(0); io.close(); }

		io.print(1);
		for (int i = 0; i < comp; i++) io.print(0);
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