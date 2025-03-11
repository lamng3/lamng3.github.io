---
layout: post
category: leetcode
title: Useful Functions for LeetCode Contests
snippet: Algorithms
tags: [algorithms, leetcode]
katex: true
---

Here are some of the useful functions to help you with LeetCode contests. Some of them have ideas based on [Algorithms for Competitive Programming](https://cp-algorithms.com).

---

### Table of Content
1. Calculate **nCr** using **Modular Multiplicative Inverse**

---

### 1. Calculate nCr using Modular Multiplicative Inverse

**nCr** is calculated as: 
$nCr = \frac{n!}{(n-r)! \, r!}$

To avoid the issues associated with large number operations and potential rounding errors when computing $n!$ divided by $(n-r)!$ and $r!$, we instead convert the divisions into multiplications using the concept of the Modular Multiplicative Inverse. Specifically, the combination $nCr$ is computed as:

$nCr = n! \times \text{inv}((n-r)!) \times \text{inv}(r!)$

where $\text{inv}(a!)$ represents the modular inverse of $a!$.

#### 1.1. Example Code

Constant `MOD` is set to $1{,}000{,}000{,}007$, a large prime number frequently used in competitive programming and algorithm design to avoid integer overflow and to guarantee the existence of modular inverses for any integer that is relatively prime to the modulus. The arrays `fact` and `invFact` are used to store the factorial values and their corresponding modular inverses, respectively.
```java
private int MOD = 1_000_000_007;
private long[] fact, invFact;
```

The following code snippet initializes a modular exponentiation function designed to compute $\text{base}^{\text{exp}}$ modulo a given modulus. This function operates in $O(\log(\text{exp}))$ ~ $O(\log(\text{N}))$ time complexity, which is achieved by repeatedly halving the exponent at each iteration (using the operation `exp >>= 1`).
```java
private long modPow(long base, int exp, int mod) {
    long result = 1;
    while (exp > 0) {
        if ((exp & 1) == 1) result = result * base % mod;
        base = base * base % mod;
        exp >>= 1;
    }
    return result;
}
```

According to Fermat's Little Theorem, for a prime number $p$, the expression $a^p - a$ is divisible by $p$. Building on this principle, we can derive that the difference $a^{p-2} - a^{-1}$ is likewise divisible by $p$. We first build the factorials. Then, we can build the inverse factorials starting with $\text{inv}(n!) = n!^{MOD-2}$ mod MOD.

```java
private void buildFactorials(int n) {
    fact = new long[n+1];
    invFact = new long[n+1];

    fact[0] = 1;
    for (int i = 1; i <= n; i++) fact[i] = fact[i-1] * i % MOD;

    invFact[n] = modPow(fact[n], MOD-2, MOD);
    for (int i = n-1; i >= 0; i--) invFact[i] = invFact[i+1] * (i+1) % MOD;
}
```

Based on the factorials and the inverse factorials calculated above, we are now able to calculate *nCr*:
```java
private long nCr(int r, int n) {
    if (r < 0 || r > n) return 0;
    return (fact[n] * invFact[r] % MOD) * invFact[n-r] % MOD;
}
```

#### 1.2. Exercises
- [3428. Maximum and Minimum Sums of at Most Size K Subsequences](https://leetcode.com/problems/maximum-and-minimum-sums-of-at-most-size-k-subsequences/description/)

---