---
layout: post
category: blog
title: 430. Flatten a Multilevel Doubly Linked List
snippet: Weekly Contest 437
tags: [algorithms, leetcode]
---

Give this problem a try [430. Flatten a Multilevel Doubly Linked List](https://leetcode.com/problems/flatten-a-multilevel-doubly-linked-list/description/)

---

### Problem Statement

Given a string `s` of length `n` and an integer `k`, determine whether it is possible to select `k` disjoint special substrings.

A **special substring** is defined as a substring where:
- **Any character present inside the substring should not appear outside it in the string.**
- **The substring is not the entire string `s`.**

All `k` substrings must be disjoint, meaning they cannot overlap.

**Return:**  
Return `true` if it is possible to select `k` such disjoint special substrings; otherwise, return `false`.

---

#### Examples

**Example 1:**

- **Input:**  
  `s = "abcdbaefab", k = 2`
- **Output:**  
  `true`
- **Explanation:**  
  We can select two disjoint special substrings: `"cd"` and `"ef"`.  
  - `"cd"` contains the characters `'c'` and `'d'`, which do not appear elsewhere in `s`.  
  - `"ef"` contains the characters `'e'` and `'f'`, which do not appear elsewhere in `s`.

**Example 2:**

- **Input:**  
  `s = "cdefdc", k = 3`
- **Output:**  
  `false`
- **Explanation:**  
  There can be at most 2 disjoint special substrings: `"e"` and `"f"`.  
  Since `k = 3`, it is impossible to find 3 disjoint special substrings, so the output is `false`.

**Example 3:**

- **Input:**  
  `s = "abeabe", k = 0`
- **Output:**  
  `true`

---

#### Constraints

- `2 <= n == s.length <= 5 * 10^4`
- `0 <= k <= 26`
- `s` consists only of lowercase English letters.

---

### Prerequisite Knowledge

- DFS
- Linked List
- Tree Traversal

---

### Example Code
```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node prev;
    public Node next;
    public Node child;
};
*/

class Solution {
    public Node flatten(Node head) {
        if (head == null) return head;
        
        Node pseudoHead = new Node(0, null, head, null);
        Node curr, prev = pseudoHead;
        
        Stack<Node> stack = new Stack<>();
        stack.push(head);

        while (!stack.isEmpty()) {
            curr = stack.pop();
            prev.next = curr;
            curr.prev = prev;

            if (curr.next != null) stack.push(curr.next); // next processed after flatten
            if (curr.child != null) {
                // flatten
                stack.push(curr.child);
                curr.child = null;
            }
            prev = curr;
        }
        
        pseudoHead.next.prev = null;
        return pseudoHead.next;
    }
}
```

#### Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(n)

---

### Sign-off

Congratulations on making it this far! Best of luck in your future competitions!