# Binary Search Trees (BST)

**Date:** 2026-03-31


Today I learned about **Trees**, specifically **Binary Search Trees (BST)**. This data structure completely blew my mind because it solves the biggest dilemmas of arrays and linked lists by combining their best features!

## 1. The Problem We Are Solving 
* **Arrays** offer contiguous memory and allow for super-fast searching (Binary Search) because we can jump to the middle. But they are fixed in size and slow to insert new data.
* **Linked Lists** are dynamic and allow for super-fast insertion. But they are incredibly slow to search ($O(n)$) because we have to check every single node one by one.

**The Solution:** A Binary Search Tree. It uses pointers (like a linked list) to be perfectly dynamic, but it organizes the data in a sorted, branching way so we can cut our search time in half at every step (like an array)!

## 2. The Anatomy of a Tree Node 
Instead of a `next` pointer, a tree node has **two** pointers: one for the left branch, and one for the right branch.

```c
typedef struct node {
    int number;             // The actual data
    struct node *left;      // Pointer to the smaller left child
    struct node *right;     // Pointer to the larger right child
} node;
```

## 3. The Golden Rule of a BST 
To make a tree a Binary Search Tree, it must follow one strict rule:
- Everything to the LEFT of a node must be LESS than the node.
- Everything to the RIGHT of a node must be GREATER than the node.

**Visualizing the Tree**
- Imagine taking a sorted sequence of numbers and pulling the middle number up by a string:

```Plaintext
               [ 50 ]   <-- ROOT
              /      \
      LEFT  /          \  RIGHT
    (Smaller)         (Larger)
          /              \
      [ 25 ]            [ 75 ]
      /    \            /    \
   [10]    [30]      [60]    [90]
```

## 4. Searching the Tree (The Magic of Recursion) 
Because of the Golden Rule, searching is incredibly efficient. If I am looking for `30`:
- Start at `50`. `30` is smaller, so go left.
- Now at `25`. `30` is larger, so go right.
- Found it! We ignored half the tree at every step.

**Implementation in C:**
```C
#include <stdbool.h>

bool search(node *tree, int number) {
    // Base Case 1: The tree is empty, or we hit a dead end
    if (tree == NULL) {
        return false;
    }
    // Recursive Case 1: Look left
    else if (number < tree->number) {
        return search(tree->left, number);
    }
    // Recursive Case 2: Look right
    else if (number > tree->number) {
        return search(tree->right, number);
    }
    // Base Case 2: We found it!
    else if (number == tree->number) {
        return true;
    }
}
```

## 5. Time Complexity
Because we eliminate half of the remaining nodes with every single step, the time complexity for searching (and inserting) drops drastically compared to a linked list.
**Search Time:** $O(\log n)$
**Caveat:** This $O(\log n)$ speed only applies if the tree is "balanced" (meaning it looks like a nice pyramid). If we insert numbers perfectly in order (like `1, 2, 3, 4, 5`), it just turns into a lopsided linked list and degrades back to $O(n)$!