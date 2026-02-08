# Pointers & Memory Management 

**Date:** 2026-02-08

Today I learned that Pointers provide a powerful alternative to passing data between functions. Instead of passing a copy of the data (Pass by Value), pointers allow me to pass the **actual variable itself** (Pass by Reference).

## 1. The Memory Model 
* **Storage vs. Memory:** Files live on the disk (HDD/SSD), but to use data, we must move it to **RAM**.
* **The Big Array:** Memory is basically a huge array of 8-bit wide bytes.
* **Addresses:** Just like an array index, every single byte in memory has a unique **address**.

## 2. What is a Pointer? 📍
A pointer is simply a data item where:
1.  The **Value** is a memory address.
2.  The **Type** describes the data located at that address.

> "Pointers are just addresses."

## 3. The Operators: `&` and `*` 

### A. The Address Extraction Operator (`&`)
This operator retrieves the address of an existing variable.
* If `x` is an `int`, then `&x` is a pointer-to-int whose value is the address of `x`.

### B. The Dereference Operator (`*`)
This operator "goes to the reference." It accesses the data at that specific memory location, allowing you to manipulate it.

* **Analogy:** Having a neighbor's address isn't enough. You need to **go to** (`*`) the address to actually interact with them

## 4. Code Example 
```c
int k;
k = 5;

int *pk;   // Declare a pointer to an integer
pk = &k;   // 'pk' now stores the address of 'k'
```
* **Visual:** `k` (value 5) is at one location. `pk` is at another location, but it points back to `k`