# Python Basics: Printing, Variables & Modules 🐍

**Date:** 2026-05-15
**Category:** Python Programming
**Tags:** #Python #Basics #Variables #Modules #Syntax

Today I officially started learning Python! Coming from C, the syntax is ridiculously clean. No semicolons `;`, no curly braces `{}`, and absolutely no memory management!

## 1. Variable Types (Dynamic Typing) 
In C, I had to declare exactly what a variable was (`int x = 5;` or `char name[20];`). 
In Python, variables are **dynamically typed**. The interpreter just *guesses* the type based on what I put inside it!

**The Core Types:**
```python
# I don't need to write 'int', 'float', or 'char'!
age = 19              # int (Integer)
gpa = 8.5             # float (Decimal)
name = "Abu Mone"     # str (String) - Notice I can just assign a whole string without arrays!
is_coding = True      # bool (Boolean) - Capital 'T' and 'F' in Python!

# To check what type a variable is, use type():
print(type(age))      # Output: <class 'int'>
```

## 2. The Mighty `print()` Statement 
Printing in Python is so much easier than `printf` in C. There are three main ways to print variables with text:

### A. Comma Separation (The Easy Way)
Python automatically adds spaces between items separated by commas.

```Python
score = 100
print("My score is", score) 
# Output: My score is 100
```
### B. Concatenation (The Strict Way)
Using the `+` sign. But beware! You cannot add a string and an integer together directly. You must cast the integer to a string first using `str()`

```Python
score = 100
print("My score is " + str(score)) 
# Output: My score is 100
```
### C. f-Strings 
This is the modern, professional way to print in Python. You put an `f` before the string, and then you can inject variables directly into the text using curly braces `{}`. It completely replaces the need for `%d`and `%s`!

```Python
name = "Jarvis"
power_level = 9000

print(f"My assistant {name} has a power level over {power_level}!")
# Output: My assistant Jarvis has a power level over 9000!
```

## 3. Modules vs. Packages 
In C, we used `#include <stdio.h>` to bring in outside code. Python has a massive ecosystem for this, broken down into Modules and Packages.

**Module:** A single `.py` file containing Python code (functions, variables).
**Package:** A whole folder (directory) containing multiple Modules grouped together.

*How to use them (`import`):**
Python comes with a "Standard Library" full of built-in modules so I don't have to reinvent the wheel.

```Python
# Example 1: Importing a whole module
import math

result = math.sqrt(64)
print(f"The square root is {result}")

# Example 2: Importing just ONE specific tool from a module
from random import randint

# Generates a random number between 1 and 10

lucky_number = randint(1, 10) 
print(f"Your lucky number is {lucky_number}")
```

---
**Key Takeaway:** Python is built for developer speed. F-strings make outputting data incredibly natural, and the module system means I can borrow code from thousands of other developers instantly using `import`!