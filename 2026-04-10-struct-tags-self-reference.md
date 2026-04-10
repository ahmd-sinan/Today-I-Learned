# Struct Tags & Self-Referencing 

**Date:** 2026-04-10

Today I learned exactly *why* we use "tags" when defining structs in C, especially when building complex data structures like Trees, Linked Lists, or Genealogy Graphs.

## 1. The Setup: A Self-Referencing Struct 
If I want to create a `person` struct that points to their two parents (who are also `person`s), the code looks like this:

```c
typedef struct person
{
    struct person *parents[2];
    char alleles[2];
} person;
```

## 2. The Problem Without a Tag 
Imagine trying to write this without the person tag at the top:
```C
typedef struct   // no tag!
{
    struct ??? *parents[2];  // what do you put here?!
    char alleles[2];
} person;
```

The typedef alias `person` at the very bottom doesn't actually exist yet — it only gets created after the `}` closes. So inside the struct body, you have no name to refer to yourself with!

## 3. Why the Tag Solves It 
The tag (`struct person`) is registered by the compiler the millisecond it sees it — before it even starts reading the body `{ }`.
```C
typedef struct person   // Tag created IMMEDIATELY
{   
    struct person *parents[2];  // Tag already exists, use it safely!
    char alleles[2];
} person;               // Typedef alias created HERE (later)
```

Inside the body, you can already use `struct person *` to self-reference because the compiler already knows the tag exists.

## 4. The Golden Rule of Struct Timing ⏱
| Name Type | Created When? |
| :--- | :--- |
| `struct person` (Tag) | Immediately, when the compiler first sees it. |
| `person` (Typedef) | Only after the `}` closes. |

The tag exists for one reason only: *to allow self-referencing inside the struct body*

## .5. When is the Tag NOT needed? 
If your struct has absolutely no pointers to itself, you don't even need the tag!
```C
typedef struct   // no tag needed 
{
    char name[50];
    int age;
} person;
```
This works perfectly fine because there is no self-referencing happening inside the `{ }`. But the moment you need `*next`, `*left`, or `*parents` pointing to the same type, you MUST use the tag!