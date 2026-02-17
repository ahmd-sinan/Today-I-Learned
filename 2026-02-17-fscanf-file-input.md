# File I/O: Reading Data with `fscanf()` 

**Date:** 2026-02-17

Today I learned about **`fscanf()`** (File Scan Formatted). It is the input counterpart to `fprintf`. It allows me to read specific data types (integers, strings, floats) directly from a file stream. `fscanf` is perfect for reading structured data (like columns in a CSV or text file), but I must be careful to match the format string exactly to the file layout.

## 1. Syntax 
```c
int fscanf(FILE *stream, const char *format, ...);
```
- `stream`: A pointer to the file we want to read from (opened with `"r"`)
- `format`: The pattern of data we expect (e.g., `"%s %d`" to read a Name followed by a Score)
- `...`: The addresses of the variables where we want to store that data (e.g., `&name`, `&score`)