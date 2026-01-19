# Standard Library Functions: Beyond the Basics 

**Date:** 2026-01-19

I was already familiar with standard header files like `<stdio.h>`, `<stdlib.h>`, `<ctype.h>`, `<string.h>`, and etc. but today I learned specifically about the powerful utility functions hidden inside `<ctype.h>`, `<math.h>`, `<stdlib.h>`, and `<string.h>`.

## 1. Character Handling: `<ctype.h>` 
These functions take a character as input and return an `int` (Boolean logic: Non-zero if True, 0 if False).

| Function | Return Type | Description |
| :--- | :--- | :--- |
| **`isalnum(c)`** | `int` | Checks if `c` is alphanumeric (A-Z, a-z, or 0-9). |
| **`isalpha(c)`** | `int` | Checks if `c` is an alphabet letter only. |
| **`isblank(c)`** | `int` | Checks if `c` is a blank space or a tab (`\t`). |
| **`isdigit(c)`** | `int` | Checks if `c` is a number digit (0-9). |
| **`isupper(c)`** | `int` | Checks if `c` is an uppercase letter. |
| **`islower(c)`** | `int` | Checks if `c` is a lowercase letter. |
| **`toupper(c)`** | `int` | **Action:** Converts `c` to Uppercase (returns the char code). |
| **`tolower(c)`** | `int` | **Action:** Converts `c` to Lowercase (returns the char code). |

### Boolean Checks Functions (Is it X or not?)
For functions like `isalpha`, `isdigit`, `islower`, etc.:
* **Return Type:** `int`
* **Return Logic:**
    * **Non-Zero:** **TRUE** (It IS that type).
    * **0:** **FALSE** (It is NOT that type).

### Conversion Functions (Change X)
For `toupper` and `tolower`:
* **Return Type:** `int` (The ASCII value of the new character).
* **Return Logic:**
    * Returns the **converted** character if valid.
    * Returns the **original** character if no conversion is possible (e.g., `toupper('1')` just returns `'1'`).

## 2. Mathematical Functions: `<math.h>` 
These functions usually handle floating-point math.
* **Return Type:** `double` (High-precision floating point number).

| Function | Return Type | Description |
| :--- | :--- | :--- |
| **`round(x)`** | `double` | Rounds `x` to the nearest integer value (e.g., 4.7 → 5.0). |
| **`pow(x, y)`** | `double` | Calculates `x` raised to the power of `y` ($x^y$). |
| **`sqrt(x)`** | `double` | Calculates the square root of `x` ($\sqrt{x}$). |

## 3. String Conversion: `<stdlib.h>` 
These functions convert "Strings" (text) into actual numbers we can do math with. "ASCII to..."

| Function | Return Type | Description |
| :--- | :--- | :--- |
| **`atoi(s)`** | `int` | Converts string to **Integer** (`"42"` → `42`). |
| **`atol(s)`** | `long` | Converts string to **Long Integer**. |
| **`atof(s)`** | `double` | Converts string to **Float/Double** (`"3.14"` → `3.14`). |

* **Return Logic:**
    * Returns the **converted number** if successful.
    * Returns **`0`** if the string does not start with a number (e.g., `atoi("foo")` returns `0`).

## 4. String Manipulation: `<string.h>` 
Functions for comparing and copying strings.

### `strcpy(dest, src)` - String Copy
* **Return Type:** `char *` (Pointer to the destination)
* **Description:** Copies the string pointed to by `src` (including the null terminator `\0`) to the buffer pointed to by `dest`.
* `dest` must be large enough to hold `src`!

### `strcmp(s1, s2)` - String Compare
* **Return Type:** `int`
* **Description:** Compares two strings character by character (using ASCII values).

**The Return Value Logic:**
| Return Value | Meaning |
| :---: | :--- |
| **< 0** (Negative) | `s1` comes **before** `s2` (e.g., "Apple" vs "Banana"). |
| **0** (Zero) | `s1` and `s2` are **identical**. |
| **> 0** (Positive) | `s1` comes **after** `s2` (e.g., "Zebra" vs "Apple"). |

---
So I don't need to write my own loops to check if a char is a number or to calculate a power. The standard library has optimized functions ready to use!