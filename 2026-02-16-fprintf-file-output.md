# File I/O: Writing Data with `fprintf()`

**Date:** 2026-02-16

Today I learned about **`fprintf()`** (File Print Formatted). It works almost exactly like `printf`, but instead of printing to the terminal, it writes the formatted text into a specific file.

## Syntax ✍️
```c
int fprintf(FILE *stream, const char *format, ...);
```

- stream: A pointer to the file we want to write to (obtained from `fopen`)
- format: The text string with placeholders (`%s`, `%d`, etc.), just like normal `printf`