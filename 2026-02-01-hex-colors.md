# Hexadecimal Colors

**Date:** 2026-02-01

Today I learned how to read those 6-digit color codes used in programming. They are actually just three separate **Hexadecimal** numbers combined into one string.

## 1. The Structure: RRGGBB 
A standard hex color code consists of 6 digits, split into three pairs.
* **Digits 1-2 (RR):** The amount of **Red**.
* **Digits 3-4 (GG):** The amount of **Green**.
* **Digits 5-6 (BB):** The amount of **Blue**.

## 2. The Math: Why `ff`? 
In an 8-bit system, the highest possible number is **255**.
In Hexadecimal:
* `0` is the lowest ($0$).
* `f` is the highest digit ($15$).
* Therefore, `ff` is the maximum value ($15 \times 16 + 15 = 255$).

So, `ff` literally means **"100% intensity"** for that color.

## 3. Color Examples 
I learned the logic behind mixing these lights:

| Hex Code | Color | Red Value | Green Value | Blue Value | Logic |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **`#ff0000`** | **Red** | **255** (`ff`) | 0 (`00`) | 0 (`00`) | Only the Red light is on. |
| **`#00ff00`** | **Green** | 0 (`00`) | **255** (`ff`) | 0 (`00`) | Only the Green light is on. |
| **`#0000ff`** | **Blue** | 0 (`00`) | 0 (`00`) | **255** (`ff`) | Only the Blue light is on. |
| **`#000000`** | **Black** | 0 (`00`) | 0 (`00`) | 0 (`00`) | All lights are **OFF**. |
| **`#ffffff`** | **White** | **255** (`ff`) | **255** (`ff`) | **255** (`ff`) | All lights are **ON** (Full Brightness). |

---
**Key Takeaway:** A computer screen uses "Additive Color." If you mix max Red, max Green, and max Blue (`#ffffff`), you get pure **White** light!