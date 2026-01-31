# ANSI Escape Codes: Coloring the Terminal 

**Date:** 2026-01-31

Today I learned how to control the terminal's text color and formatting using **ANSI Escape Codes**. These are special sequences of characters that the terminal interprets as commands rather than text to print.

## 1. The Anatomy of a Code 
In C or a Linux terminal, the escape code usually follows this structure: `\033[ (code) m`.
- ANSI = American National Standards Institute

* **`\033`**: The **Escape** character (represented in Octal).
* **`[`**: The Control Sequence Introducer.
* **`(code)`**: The number representing the color or style (e.g., `31` for Red).
* **`m`**: The character that tells the terminal "this is a graphic mode change".

## 2. Common Color Codes 
Here is a cheat sheet for the most used text colors:

| Color | Code | Example |
| :--- | :--- | :--- |
| **Reset** | 0 | `"\033[0m"` (Stops the color) |
| **Bold** | 1 | `"\033[1m"` |
| **Red** | 31 | `"\033[31m"` |
| **Green** | 32 | `"\033[32m"` |
| **Yellow** | 33 | `"\033[33m"` |
| **Blue** | 34 | `"\033[34m"` |
| **Magenta** | 35 | `"\033[35m"` |
| **Cyan** | 36 | `"\033[36m"` |

## 3. How to Use it in C
Typing `\033[31m` inside every `printf` is messy and hard to read. A pro tip is to use **Macros** (`#define`) to make the code readable.

```c
#include <stdio.h>

// Define colors for easy use
#define RED     "\033[31m"
#define GREEN   "\033[32m"
#define YELLOW  "\033[33m"
#define RESET   "\033[0m"

int main(void) {
    // We can concatenate strings automatically in C
    printf(RED "This text is Red!" RESET "\n");
    
    printf(GREEN "This is Green, perfect for a 'Success' message!" RESET "\n");
    
    printf(YELLOW "Warning: This is Yellow." RESET "\n");

    return 0;
}
