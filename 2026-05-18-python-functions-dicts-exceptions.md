# Python Power-Ups: Functions, Exceptions & Dictionaries 🐍

**Date:** 2026-05-18

Today I leveled up in Python by exploring how it handles modular code, errors, and complex data structures. Seeing how Python replaces C's arrays and structs is mind-blowing!

## 1. Functions (`def`) 
In C, I had to declare function prototypes at the top of the file so the compiler knew they existed. In Python, I just define them using `def`. 

By defining a `main()` function and calling it at the very bottom of the script, Python reads all the custom functions first, avoiding any "function not found" errors!

```python
def main():
    meow(3)

def meow(n):
    for i in range(n):
        print("meow")

# Call main to kick off the program
main()
```

## 2. Exceptions (Catching Crashes) 
If I use `int(input())` and the user types `"cat"`, the program will completely crash with a `ValueError`. To prevent this, Python uses `try` and `except` blocks to catch the error gracefully.

```Python
try:
    n = int(input("Integer: "))
    print(f"Integer: {n}")
except ValueError:
    print("Not an integer! Please type a number next time.")
```

## 3. Lists (Dynamic Arrays) & Math 
In C, calculating an average required a loop to add everything together. Python lists are incredibly powerful and come with built-in functions like `sum()` and `len()`. I don't even need to declare a fixed size!

```Python
scores = []

# Dynamically appending to the list
for i in range(3):
    number = int(input(f"Score {i+1}: "))
    scores.append(number)

# Python does the math for us!
average = sum(scores) / len(scores)

# Using .2f in an f-string rounds it to 2 decimal places
print(f"Average: {average:.2f}") 
```

## 4. Dictionaries (Key-Value Pairs) 
A Dictionary in Python is a collection of key-value pairs separated by commas. Instead of using numerical indexes (like `array[0]`), I can look up data using a string as the key!

```Python
# A simple phonebook dictionary
people = {
    "Amit": "+91-98765-43210",
    "Priya": "+91-12345-67890",
    "Rahul": "+91-11223-33445"
}

name = input("Name: ")

# Checking if a key exists in the dictionary
if name in people:
    number = people[name]
    print(f"Found: {number}")
else:
    print("Not found")
```

## 5. List of Dictionaries (The Ultimate Combo) 
I can put multiple dictionaries inside a single list. This completely replaces the need for parallel arrays!

When iterating through a list of dictionaries with a `for` loop, the iteration variable isn't a number (`0`, `1`, etc.)—it is the actual dictionary object itself!

```Python
people = [
    {"name": "Amit", "number": "+91-98765-43210"},
    {"name": "Priya", "number": "+91-12345-67890"},
    {"name": "Rahul", "number": "+91-11223-33445"}
]

name = input("Name: ")

for person in people:
    if person["name"] == name:
        print(f"Found: {person['number']}")
        break
else:
    print("Not found")
```

## 6. The Big Realization: C Structs vs. Python Dicts 

This is the ultimate mapping of my C knowledge to Python. A Python dictionary is essentially doing the exact same job as a C `struct`, just without the strict typing!

**In C (Structs):**

```C
typedef struct {
    string name;
    int age;
    string city;
} person;

person people[3];
people[0].name = "Alice";
people[0].age = 30;
// Accessed via dot notation
```

**In Python (Dictionaries):**

```Python
people = {"name": "Alice", "age": 30}
# Accessed via keys
print(people["name"])
```