# Strings: Arrays vs. Pointers (`char[]` vs `char*`) 

**Date:** 2026-02-03

Today I learned that while strings in C are always sequences of characters ending in `\0`, how we declare them changes where they live in memory and whether we can change them

## The Array Method: `char a[]` 
```c
char a[] = "Hi!";
```

- **What happens**: The program creates a copy of the string "Hi!" and puts it inside a completely new array on the Stack
- Editable

- I can change it: `a[0] = 'B';` works fine
- The string becomes "Bi!"
- Memory: The variable a is the array itself

## The Pointer Method: `char *p`
```c
char *p = "Hi!";
```

- **What happens**: The string literal "Hi!" is stored in a special "Read-Only Data" segment of memory. The pointer p simply stores the address of that string
- Read-Only (Immutable)

- If I try `p[0] = 'B';`, the program will crash (Segmentation Fault)
- I am trying to write to read-only memory!
- Memory: The variable p is just a small pointer (8 bytes) pointing to a shared string somewhere else

## Comparison 
| Feature | `char a[] = "Hi!";` | `char *p = "Hi!";` |
| :--- | :--- | :--- |
| **Storage** | Stored on the Stack (Modifiable) | Points to Read-Only Memory (Literal Pool) |
| **Editing** | Can change characters (`a[0] = 'X'`) | CRASH if you try to change characters |
| **Reassignment** | Cannot point to a new string (`a = "Bye"` is error) | Can point to a new string (`p = "Bye"` is allowed) |
| **Size `(sizeof)`** | Returns size of the array (e.g., 4 bytes) | Returns size of the pointer (e.g., 8 bytes) |

If I need to modify the string (like `toupper`), I MUST use an array (`char a[]`). If I just need to read/print it, a pointer (`char *p`) saves memory