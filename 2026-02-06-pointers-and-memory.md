# Pointers, Memory & The Truth About Strings

**Date:** 2026-02-04

Up until now, I thought of variables as "boxes" that hold values. But in low-level C, variables are actually **locations** in memory.

* **Value:** What is inside the box (e.g., `50`).
* **Address:** Where the box is located in memory (e.g., `0x7ffe00b`).
* **Pointer:** A variable designed specifically to hold an *address*, not a value.

---

## The "String" Illusion
One of the biggest realizations is that **the `string` data type does not actually exist in C.** It is an abstraction.

### The Reality: `char *`
A string is simply a pointer to the **first character** in a sequence of characters.

```c
// This is not a "string object".
// This is a pointer to the letter 'H'.
char *text = "Hello";
```

- `char *`: Tells the computer, "Go to this address; you will find a character there"
- **The Chain Reaction**: The computer prints the character at that address, then moves to the next address, and the next, until it hits the **Null Terminator**

### The Null Terminator (`\0`)
- `\0` (All bits zero) marks the end of the string.
- If you overwrite the `\0`, the program will keep reading into garbage memory (buffer overflow)

## Pointers & Data Types
A pointer is just an address, but the type matters because it tells the computer "how big" the house at that address is
| Syntax | Meaning | Step Size (Pointer Arithmetic) | 
| :--- | :--- | :--- |
| char * |Address of a Character | Moves 1 byte at a time |
| int * | Address of an Integer | Moves 4 bytes at a time |
| FILE * | Address of a File Stream | Moves by file structure size |


You must match the pointer type to the variable type
``` C
int age = 20;
int *p = &age;  // Correct: int* points to int

char grade = 'A';
char *c = &grade; // Correct: char* points to char
``` 
- `&` (Address of): "Get me the address of this variable"
- `*` (Dereference): "Go to this address and show me what is inside"
