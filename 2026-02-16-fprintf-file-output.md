# File I/O: Writing Data with `fprintf()`

**Date:** 2026-02-16

Today I learned about **`fprintf()`** (File Print Formatted). It works almost exactly like `printf`, but instead of printing to the terminal, it writes the formatted text into a specific file.
`fprintf` can use to generate reports, save game states, or create logs that persist even after the program turns off.

## Syntax 
```c
int fprintf(FILE *stream, const char *format, ...);
```

- stream: A pointer to the file we want to write to (obtained from `fopen`)
- format: The text string with placeholders (`%s`, `%d`, etc.), just like normal `printf`

## Basic Example 
To write "Hello, World!" into a text file named `log.txt`:
```c
#include <stdio.h>

int main(void)
{
    // 1. Open the file for writing ("w")
    FILE *file = fopen("log.txt", "w");

    // 2. Check if file opened successfully
    if (file == NULL)
    {
        return 1;
    }

    // 3. Print TO THE FILE instead of the screen
    char *name = "Abu";
    fprintf(file, "Hello, %s!\n", name);
    fprintf(file, "System Status: %d%%\n", 100);

    // 4. Always close the file to save changes!
    fclose(file);
}
```
## The "Standard" Streams 
I learned a fascinating detail:
- `stdout` (Standard Output) is actually just a special file stream that represents the terminal screen.
Therefore, `printf("Hi");` is logically equivalent to `fprintf(stdout, "Hi");`

## Error Handling 
- Just like `printf`, `fprintf` returns the number of characters written.
- If it returns a negative number, it means a write error occurred (e.g., disk full)