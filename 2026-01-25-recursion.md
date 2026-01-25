# Recursion: Functions Calling Themselves 

**Date:** 2026-01-25
**Category:** Algorithms & C Programming
**Tags:** #Recursion #Algorithms #Stack #Factorial

Today I learned about **Recursion**, a programming technique where a function calls *itself* to solve a problem. It breaks a large problem down into smaller, identical sub-problems.

## 1. The Anatomy of Recursion 
Every recursive function **MUST** have two parts, or it will run forever and crash the computer (Stack Overflow).

1.  **The Base Case (The Stop Sign):**
    * The condition where the function stops calling itself.
    * It returns a solid value instead of another function call.
2.  **The Recursive Case (The Loop):**
    * The part where the function calls itself again, but with a *smaller* version of the original problem.

## 2. Classic Example: Factorial (!) 
Mathematically, $5! = 5 \times 4 \times 3 \times 2 \times 1$.
* We can say: $5! = 5 \times 4!$
* And $4! = 4 \times 3!$
* ...until we hit $1!$ (which is 1).

### Code Implementation (Standard C)
```c
#include <stdio.h>

int factorial(int n);

int main(int argc, char *argv[])
{
    int result = factorial(5);
    printf("Result: %d\n", result); // Output: 120
    return 0;
}

int factorial(int n)
{
    // 1. Base Case: If n is 1, stop and return 1.
    if (n == 1)
    {
        return 1;
    }
    
    // 2. Recursive Case: n * factorial of (n-1)
    else
    {
        return n * factorial(n - 1);
    }
}
```

### 3. Visualizing the Call Stack 
When factorial(3) runs, it doesn't finish immediately. It pauses and adds a new "frame" to the memory stack.
- 1. Start: factorial(3) calls...
- 2. Pause: 3 * factorial(2) calls...
- 3. Pause: 2 * factorial(1) calls...
- 4. Base Case Hit: factorial(1) returns 1.
- 5. Unwind: $2 \times 1 = 2$
- 6. Unwind: $3 \times 2 = 6$ (Final Answer).

### 4. Recursion vs. Iteration (Loops) ⚔️
| **Feature** | **Recursion** 🌀 | **Iteration (While/For)** 🔁 |
| :--- | :--- | :--- |
| *Code Style* | Elegant, often fewer lines of code | Can be longer, but straightforward |
| *Memory* | Uses more memory (Call Stack builds up) | Efficient (Uses constant memory) |
| *Risk* | Risk of Stack Overflow if base case is missed | Risk of Infinite Loop (CPU hangs) |
| *Use Case* | Tree structures, sorting (Merge Sort) | Simple counting, navigating arrays |

Recursion is like "Russian Dolls" (Matryoshka). You open one to find a smaller version of the same doll inside, until you reach the tiniest solid doll at the center (the Base Case).