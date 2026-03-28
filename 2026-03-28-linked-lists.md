# Linked Lists: Breaking Free from Arrays 

**Date:** 2026-03-28

Today I learned about **Linked Lists**. This solves the biggest problem with arrays: fixed capacity. Instead of reserving a massive block of memory side-by-side, a linked list scatters data wherever there is free space in RAM and uses pointers to "link" them together!

## 1. The Anatomy of a Node 
A linked list is made up of individual blocks called **Nodes**. 
Every node contains exactly two things:
1.  **The Data:** The actual information (like an `int`, or a `struct Student`).
2.  **The Pointer (`next`):** A pointer storing the memory address of the *next* node in the chain.

```c
typedef struct node {
    int number;           // The data
    struct node *next;    // The map to the next piece of data
} node;
```

## 2. Big O Time Complexity ⏱
Because the data isn't stored right next to each other in memory, we lose the ability to use array indexes (like `arr[5]`).
- **Search / Access:** $O(n)$ - To find the 100th item, we MUST start at the 1st item and follow the pointers 100 times. We can't just jump there.
- **Insertion (at beginning):** $O(1)$ - Extremely fast! We just create a new node and point it to the current first node.
- **Deletion:** $O(n)$ to find the item, but $O(1)$ to actually reroute the pointers and delete it.

## 3. Main Operations 
- **Insertion:** Adding a new node (Head, Tail, or Middle).
- **Deletion:** Removing a node and carefully rewiring the next pointer of the previous node so the chain doesn't break!
- **Search:** Traversing the list with a loop to find specific data.

## 4. My Implementation (With Memory Cleanup!) 
Here is the code to dynamically build a linked list by inserting new numbers at the front (the Head), printing them using a clean `for` loop, and then safely freeing the memory to prevent leaks.
```c
#include <stdio.h>
#include <stdlib.h>

typedef struct node {
    int number;
    struct node *next;
} node;

int main() {
    node *list = NULL; // The Head pointer (starts empty)

    // --- 1. INSERTION ---
    for (int i = 0; i < 3; i++) {
        node *n = malloc(sizeof(node)); // dynamically allocate memory
        if (n == NULL) return 1;

        printf("Enter a number: ");
        scanf("%i", &n->number);
        
        // Point the new node to whatever the list is currently pointing at
        n->next = list; 
        
        // Update the Head to be this new node
        list = n; 
    }

    // --- 2. TRAVERSAL (SEARCH/PRINT) ---
    printf("\nYour Linked List:\n");
    // Traverse the list and print the numbers
    for (node *ptr = list; ptr != NULL; ptr = ptr->next) {
        printf("%i\n", ptr->number);
    }

    // --- 3. DELETION / MEMORY CLEANUP (Crucial!) ---
    // Free the memory allocated for the nodes in the list
    node *ptr = list;
    while (ptr != NULL) {
        node *temp = ptr; // Safely store the current node in a temporary variable so we don't lose it!
        ptr = ptr->next;  // Jump to the next node BEFORE we destroy the bridge
        free(temp);       // Now it's safe to free the memory allocated for the old current node
    }

    return 0;
}
```

## 5. Extra Things I Learned 
- **The Head Pointer:** The variable list isn't actually a node itself; it is just a pointer to the first node. If you lose the Head pointer, you lose the entire list in memory!
- **Temporary Pointers:** When freeing memory or deleting a node, you MUST use a temporary pointer (`temp) to hold the address of the next node. If you `free(ptr)` first, you destroy the map to the rest of your data!
- **Singly vs. Doubly:** This is a "Singly" linked list (one-way street). A "Doubly" linked list has a `prev` pointer too, letting you go backwards!