# Debugging Memory Leaks with Valgrind 

**Date:** 2026-02-12

Today I learned about **Memory Leaks** and how to catch them using a powerful Linux tool called **Valgrind**.

## 1. What is a Memory Leak? 
When we allocate memory manually (using `malloc`), it stays occupied until we explicitly release it (using `free`)
* If we lose the pointer to that memory without freeing it, that chunk of RAM is "lost" or "leaked."
* The operating system can't use it, and our program can't reach it.
* **Consequence:** If a program runs for a long time (like a server or game), these small leaks add up and eventually crash the system!

## 2. Installing Valgrind 
Valgrind is not always installed by default on Linux/Ubuntu. To install valgrind:

```bash
sudo apt update
sudo apt install valgrind
```

## How to Use It 
Instead of running the program directly (./program), we run it inside Valgrind.

```Bash
valgrind ./program
```

### Understanding the Output
Valgrind analyzes every byte of memory access.
* Good Output: "All heap blocks were freed -- no leaks are possible." ✅
* Bad Output: "definitely lost: 40 bytes in 1 blocks." ❌
This means I `malloc`'d 40 bytes but forgot to `free` them before the program ended

## Helpful Flags 
To see exactly which line of code caused the leak, we can add flags:

```Bash
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes ./program
```
For every `malloc`, there must be a matching `free`. Valgrind helps me enforce this rule!!!
