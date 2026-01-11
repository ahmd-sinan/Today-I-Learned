# Algorithms & Pseudocode 🧩

**Date:** 2026-01-11  
**Category:** CS Fundamentals  

Problem-solving is central to programming. Today I learned about defining steps to solve problems (Algorithms) and planning code logic (Pseudocode).

## 1. Algorithms & Big-O Notation 
An **Algorithm** is a step-by-step set of instructions to solve a problem. 

Using the "Phone Book Search" problem (finding one name among 100 pages), we can visualize efficiency using **Big-O notation** (worst-case scenario speed).

| Approach | Description | Big-O (Speed) |
| :--- | :--- | :--- |
| **Linear Search** | Flip 1 page at a time. Slowest. | $O(n)$ |
| **Fast Linear** | Flip 2 pages at a time. Twice as fast, but still linear. | $O(n/2)$ |
| **Binary Search** | Open middle, cut problem in half repeatedly. Fastest. | $O(\log n)$ |

> **Note:** $O(\log n)$ is powerful. Even if you double the problem size, it only takes **one** extra step to solve.

## 2. Pseudocode 
Pseudocode is human-readable instruction used to plan logic before writing actual syntax. It bridges the gap between human thought and machine code.

**Example (Binary Search Logic):**
```text
1  Pick up phone book
2  Open to middle of phone book
3  Look at page
4  If person is on page
5      Call person
6  Else if person is earlier in book
7      Open to middle of left half of book
8      Go back to line 3
9  Else if person is later in book
10     Open to middle of right half of book
11     Go back to line 3
12 Else
13     Quit