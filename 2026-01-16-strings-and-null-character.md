# Strings & The Null Character (`\0`) 

**Date:** 2026-01-16

Today I learned that "Strings" in C are technically just arrays of characters. But unlike a normal array of numbers, strings need a special way to mark their ending.

## 1. Strings are Arrays 
When we write `char s[] = "HI!";` ( or `char *s = "HI!";`), the computer is actually storing an array of characters.
Since each `char` is 1 byte, this string takes up **4 bytes**, not 3!

### The Visual Memory Map
| Index | `[0]` | `[1]` | `[2]` | `[3]` |
| :---: | :---: | :---: | :---: | :---: |
| **Char** | 'H' | 'I' | '!' | **`\0`** |
| **ASCII** | 72 | 73 | 33 | **0** |

## 2. The Sentinel: `\0` (NUL) 
* **Symbol:** `\0` (Backslash zero)
* **ASCII Value:** 0 (All bits are zero: `00000000`)
* **Purpose:** It acts as a "Stop Sign." It tells functions like `printf` where the string ends.

### Why is it important?
Computers function on "dumb" logic. If we tell to a computer "Print this string" but don't provide the `\0`, the computer will:
1. Print 'H'
2. Print 'I'
3. Print '!'
4. **Keep printing whatever random garbage data** exists in the memory slots next to it until it crashes or hits a zero by accident.

## 3. String Length Calculation 
Because of this design, calculating the length of a string is a simple loop:
```c
int n = 0;
while (text[n] != '\0') // Keep going until we hit the stop sign
{
    n++;
}
// n is now the length
