# Binary File I/O: Reading Raw Data with `fread()` 

**Date:** 2026-02-23

Today I learned about **`fread()`**. This is the exact opposite of `fwrite()`. Instead of translating text character by character (like `fscanf` does), `fread()` grabs massive blocks of raw binary data from a file and dumps it straight into a variable or array in memory. fread() is incredibly fast because it skips all data translation. It just lifts raw bytes off the disk and drops them directly into your program's RAM.

## The Syntax 
The syntax is identical to `fwrite()`, but the direction of data flow is reversed. It asks: *"Where am I putting this data, how big is each chunk, how many chunks, and what file am I reading from?"*

```c
size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream);
```

- `ptr`: A pointer to the memory location where you want to store the read data (e.g., an empty array or struct).
- `size`: The size in bytes of one element (sizeof(int), sizeof(char), etc.).
- `nmemb`: The number of elements you want to read.
- `stream`: The FILE * pointer we are reading from (must be opened in "rb" - read binary mode).

## Code Example: Loading an Array 
Let's read the exact same array of 5 integers we saved in the last lesson!

```C
#include <stdio.h>

int main(void)
{
    // Create an empty array to hold the incoming data
    int loaded_numbers[5];

    // 1. Open file in "rb" (Read Binary) mode
    FILE *file = fopen("data.bin", "rb");
    if (file == NULL) return 1;

    // 2. Read the entire array from the file at once
    // ptr: where to put the data
    // size: size of one integer
    // nmemb: we want 5 of them
    // stream: our file
    fread(loaded_numbers, sizeof(int), 5, file);

    // 3. Prove it worked!
    for (int i = 0; i < 5; i++)
    {
        printf("Number %d: %d\n", i, loaded_numbers[i]);
    }

    // 4. Close the file
    fclose(file);
    return 0;
}
```

## The Return Value 
fread returns the number of items successfully read.
If you ask it to read 5 items, and it returns 5, you are good!
If it returns a number less than what you asked for, it means you either hit the End of File (EOF) or an error occurred.

### Why is this useful?
Because we can use fread inside a while loop to read a file chunk by chunk until it's empty!

```C
// Example: Reading a file 1 byte at a time until the end
uint8_t buffer;
while (fread(&buffer, sizeof(uint8_t), 1, file) == 1)
{
    // Do something with the byte
}
```
