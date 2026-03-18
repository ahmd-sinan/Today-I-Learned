# Abstract Data Structures: Stacks & Queues 

**Date:** 2026-03-18

Today I learned about abstract data structures, specifically **Queues** and **Stacks**. These are standardized ways of organizing and accessing data in memory, each with very different rules!

## Queues (First-In, First-Out / FIFO) 
A Queue works exactly like a line at an amusement park. The first person to join the line is the first person who gets to go on the ride. 

* **Rule:** FIFO (First In, First Out).
* **Actions:**
  * **`enqueue`**: An item joins the back of the queue.
  * **`dequeue`**: An item is removed from the front of the queue.
* **Real-World Uses:** Printer job lines (printing documents in the order they were sent), server request handling, or matchmaking lobbies in multiplayer games.

**C Implementation (Array-Based):**
```c
const int CAPACITY = 50;

typedef struct {
    person people[CAPACITY];
    int size;
} queue;
```
Notice that `CAPACITY` is the maximum limit, but `size` tracks how many people are actually in the queue right now.

## Stacks (Last-In, First-Out / LIFO) 
A Stack contrasts completely with a Queue. It works exactly like a stack of trays in a dining hall. You can't pull a tray from the bottom; you must take the one that was placed on top most recently.

- **Rule:** LIFO (Last In, First Out).
- **Actions:**
    - `push`: Place an item on the top of the stack.
    - `pop`: Remove the item from the top of the stack.
- **Real-World Uses:** The "Undo" button in text editors (Ctrl+Z pops the last action), your web browser's "Back" button, and managing function calls in RAM (the Call Stack).

**C Implementation (Array-Based):**
```C
const int CAPACITY = 50;

typedef struct {
    person people[CAPACITY];
    int size;
} stack;
```
Structurally, the C code looks identical to the queue! The difference is entirely in the logic of how we write the `push/pop` vs `enqueue/dequeue` functions.

## The Fatal Flaw: Static Capacity 
While the array-based approach above works, it has a massive limitation: the capacity is predetermined and fixed (`const int CAPACITY = 50;`).

- **Oversizing (Wasted RAM):** If we set capacity to 5000 just to be safe, but only ever queue 5 people, we are wasting 4995 slots of memory!
- **Undersizing (Stack Overflow):** If we set capacity to 50 and 51 people show up, our program crashes.

## The Next Step: Dynamic Structures 
It would be much better if our stack or queue could be dynamic—able to grow and shrink exactly as items are added or removed.

- To solve this, we have to abandon static arrays. Instead, we use Pointers and dynamic memory allocation (`malloc` and `free`) to link individual pieces of data together only when we need them!