# Binary File I/O: Writing Raw Data with `fwrite()` 

**Date:** 2026-02-18

Today I learned about **`fwrite()`**. While `fprintf` writes formatted *text* (like writing the number `100` as the characters `'1'`, `'0'`, `'0'`), `fwrite` takes a block of memory and dumps the exact raw bytes straight into a file. `fwrite()` is literally just a "copy-paste" from RAM to the Hard Drive. It is the most efficient way to save data exactly as the computer sees it.

## Text vs. Binary 
* **Text (`fprintf`)**: Human-readable. Good for logs and `.txt` files.
* **Binary (`fwrite`)**: Machine-readable. Good for saving complex data (like audio files, images, or game save states). You open the file with `"wb"` (write binary) instead of just `"w"`.

## The Syntax 
`fwrite` takes 4 arguments. It basically asks: *"Where is the data, how big is each piece, how many pieces, and where is it going?"*

```c
size_t fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream);
```
- `ptr`: A pointer to the memory location where your data is stored (e.g., an array or a struct)
- `size`: The size in bytes of one element (we usually use `sizeof()` for this)
- `nmemb`: The number of elements you want to write
- `stream`: The `FILE *` pointer where the data will be saved

## Code Example: Saving an Array 
Instead of writing a loop to print every number, fwrite can save a whole array in one line!

```C
#include <stdio.h>

int main(void)
{
    // The data we want to save
    int numbers[5] = {10, 20, 30, 40, 50};

    // 1. Open file in "wb" (Write Binary) mode
    FILE *file = fopen("data.bin", "wb");
    if (file == NULL) return 1;

    // 2. Write the entire array to the file at once
    // ptr: array name (which is a pointer!)
    // size: size of one integer
    // nmemb: 5 integers total
    // stream: our file
    fwrite(numbers, sizeof(int), 5, file);

    // 3. Close the file
    fclose(file);
    return 0;
}
```

## Why is `fwrite` so powerful?
- Speed: It is much faster than `fprintf` because the computer doesn't have to translate numbers into text characters. It just copies the bytes.
- Efficiency: It takes up less disk space.
- Structs: I can pass a pointer to a custom `struct` (like `Student`) and save all their data (name, age, marks) in one single disk operation!

## The Return Value 
`fwrite` returns the number of items successfully written (not the number of bytes). If the return value is less than the `nmemb` argument you passed, it means an error occurred (like running out of disk space)
