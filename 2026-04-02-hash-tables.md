# Hash Tables: The Quest for $O(1)$ ⚡

**Date:** 2026-04-02

Today I learned about **Hash Tables**, which are a fantastic combination of both arrays and linked lists. They attempt to achieve the "holy grail" of programming: **Constant Time**, or $O(1)$.

## 1. What is a Hash Table? 
When implemented in C code, a hash table is simply an **array of pointers to nodes**. 

Instead of searching through an array one by one ($O(n)$) or dividing a tree in half ($O(\log n)$), a hash table takes a value and runs it through an algorithm to instantly calculate its exact array index. 

## 2. The Hash Function 
A hash function is an algorithm that reduces a larger value (like a string) to something small and predictable (an integer). 

For example, if we hash the word "Luigi", the function might process the first letter 'L' and output the number `11`. 

```c
#include <ctype.h>

// A simple hash function that returns the alphabetical index (0-25)
unsigned int hash(const char *word) {
    // toupper('L') is 76 in ASCII. 'A' is 65.
    // 76 - 65 = 11. The index is 11!
    return toupper(word[0]) - 'A';
}
```
Notice how the hash function returns the value of `toupper(word[0]) - 'A'`

## 3. Collisions & Linked Lists
What happens if we hash "Luigi" (index 11) and then hash "Link" (also index 11)? This is called a Collision.
- Collisions occur when you add values to the hash table, and something already exists at that hashed location.
- The Solution: This is where the linked list comes in! Instead of overwriting "Luigi", we simply append "Link" to the end of the linked list located at index 11.

```Plaintext
Index 11:  [ Luigi ] -> [ Lakitu ] -> [ Link ] -> NULL
```

## 4. The Programmer's Trade-off
Collisions can be reduced by writing better hash algorithms. For example, instead of just using the first letter (an array of 26), we could hash the first three letters (Laa, Lab, Lac...).

As the programmer, I have to make a decision:
- Use more memory: Create a massive hash table to reduce collisions and keep search times close to $O(1)$.
- Use less memory: Create a smaller hash table, but accept that many words will collide into long linked lists, increasing the search time towards $O(n)$.
---
**Key Takeaway:** A Hash Table gives me the instant random-access speed of an Array, combined with the infinite growth potential of a Linked List. The efficiency entirely depends on how clever my Hash Function is!