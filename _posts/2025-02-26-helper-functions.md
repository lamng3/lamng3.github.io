---
layout: post
category: blog
title: Useful Functions for LeetCode Contests
snippet: Algorithms
tags: [algorithms, leetcode]
katex: true
---

Here are some of the useful functions to help you with LeetCode contests. Some of them have ideas based on [Algorithms for Competitive Programming](https://cp-algorithms.com).

---

### Table of Content
1. Calculate **nCr** using **Modular Multiplicative Inverse**

### 1. Calculate nCr using Modular Multiplicative Inverse

**nCr** is calculated as: 
$$
nCr = \frac{n!}{(n-r)! \, r!}
$$

To avoid the issues associated with large number operations and potential rounding errors when computing $n!$ divided by $(n-r)!$ and $r!$, we instead convert the divisions into multiplications using the concept of the Modular Multiplicative Inverse. Specifically, the combination $nCr$ is computed as:

$$
nCr = n! \dot \text{inv}((n-r)!) \dot \text{inv}(r!)
$$

where $\text{inv}(a!)$ represents the modular inverse of $a!$.

#### Code
```java

private int MOD = 1_000_000_007;
private long[] fact, invFact;

private void buildFactorials(int n) {
    fact = new long[n+1];
    invFact = new long[n+1];

    fact[0] = 1;
    for (int i = 1; i <= n; i++) fact[i] = fact[i-1] * i % MOD;

    invFact[n] = modPow(fact[n], MOD-2, MOD);
    for (int i = n-1; i >= 0; i--) invFact[i] = invFact[i+1] * (i+1) % MOD;
}

private long modPow(long base, int exp, int mod) {
    long result = 1;
    while (exp > 0) {
        if ((exp & 1) == 1) result = result * base % MOD;
        base = base * base % MOD;
        exp >>= 1;
    }
    return result;
}

private long nCr(int r, int n) {
    if (r < 0 || r > n) return 0;
    return (fact[n] * invFact[r] % MOD) * invFact[n-r] % MOD;
}

```

#### Exercises
- [3428. Maximum and Minimum Sums of at Most Size K Subsequences](https://leetcode.com/problems/maximum-and-minimum-sums-of-at-most-size-k-subsequences/description/)
