---
layout: post
category: blog
title: 430. Flatten a Multilevel Doubly Linked List
snippet: Daily Practice
tags: [algorithms, leetcode, medium]
---

Give this problem a try [430. Flatten a Multilevel Doubly Linked List](https://leetcode.com/problems/flatten-a-multilevel-doubly-linked-list/description/)

---

### Problem Statement

You are given a doubly linked list, which contains nodes that have a **next** pointer, a **previous** pointer, and an additional **child** pointer. This child pointer may or may not point to a separate doubly linked list, also containing these special nodes. These child lists may have one or more children of their own, and so on, to produce a multilevel data structure as shown in the example below.

Given the **head** of the first level of the list, flatten the list so that all the nodes appear in a single-level, doubly linked list. Let `curr` be a node with a child list. The nodes in the child list should appear after `curr` and before `curr.next` in the flattened list.

Return the head of the flattened list. The nodes in the list must have all of their child pointers set to `null`.

#### Examples

**Example 1**

- **Input:** 
  `head = [1,2,3,4,5,6,null,null,null,7,8,9,10,null,null,11,12]`

- **Output:** 
  `[1,2,3,7,8,11,12,9,10,4,5,6]`

**Example 1**

- **Input:** 
  `head = [1,2,null,3]`

- **Output:** 
  `[1,3,2]`

**Example 3**

- **Input:** 
  `head = []`

- **Output:** 
  `[]`

#### Constraints

- The number of Nodes will not exceed 1000.
- `1 <= Node.val <= 10^5`

---

### Prerequisite Knowledge

- DFS
- Linked List
- Tree Traversal

---

### Example Code
```java
class Solution {
    public List<String> fullJustify(String[] words, int maxWidth) {
        int n = words.length;
        List<String> answer = new ArrayList<>();
        StringBuilder sb = new StringBuilder();

        int curWidth = 0;
        int wordCount = 0;
        for (int i = 0; i < n; i++) {
            int candWidth = words[i].length();
            if (curWidth + candWidth + wordCount <= maxWidth) {
                curWidth += candWidth;
                wordCount++;
            }
            else {
                int extraSpaces = maxWidth - curWidth;
                if (wordCount == 1) {
                    sb = new StringBuilder();
                    sb.append(words[i-1]);
                    for (int j = 0; j < extraSpaces; j++) sb.append(" ");
                    answer.add(sb.toString());
                }
                else {
                    sb = new StringBuilder();
                    // distribute extraSpaces to wordCount - 1
                    int[] alloc = new int[wordCount-1];
                    extraSpaces -= (wordCount - 1);
                    for (int j = 0; j < wordCount - 1; j++) alloc[j] = 1;
                    
                    // allocate remaining extraSpaces
                    while (extraSpaces > 0) {
                        for (int j = 0; j < wordCount - 1; j++) {
                            alloc[j]++;
                            extraSpaces--;
                            if (extraSpaces == 0) break;
                        }
                    }

                    for (int j = i - wordCount; j < i - 1; j++) {
                        sb.append(words[j]);
                        for (int k = 0; k < alloc[j - (i - wordCount)]; k++) sb.append(" ");
                    }
                    sb.append(words[i-1]);
                    answer.add(sb.toString());
                }
                curWidth = candWidth;
                wordCount = 1;
            }
        } 

        int extraSpaces = maxWidth - curWidth - (wordCount - 1);
        sb = new StringBuilder();
        for (int i = n - wordCount; i < n-1; i++) sb.append(words[i]).append(" ");
        sb.append(words[n-1]);
        for (int i = 0; i < extraSpaces; i++) sb.append(" ");
        answer.add(sb.toString());

        return answer;
    }
}
```

#### Complexity Analysis
- Time Complexity: O(n)
- Space Complexity: O(n)

---

### Sign-off

Congratulations on making it this far! Best of luck in your future competitions!