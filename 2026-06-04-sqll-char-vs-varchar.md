# SQL Strings: `CHAR` vs `VARCHAR` 

**Date:** 2026-06-04
**Category:** Databases / SQL
**Tags:** #SQL #Databases #DataTypes #Strings #Performance

Today I learned about the crucial differences between the two main ways to store text in a SQL database: `CHAR` and `VARCHAR`. Choosing the wrong one can either waste massive amounts of storage or slow down database performance!

## 1. The `CHAR` Data Type (Fixed-Length)
Unlike in C, the `CHAR` data type in SQL does not refer to a single character. Rather, it is a fixed-length string. In most relational databases, including MySQL, you actually specify the fixed-length as part of the type definition, e.g. `CHAR(10)`.

### How it works under the hood:
If you define a column as `CHAR(10)` and you insert the name `'Ali'`, the database doesn't just store 3 characters. It permanently reserves 10 spaces and pads the rest with blanks: `'Ali       '`. 

*   **Pros:** It is incredibly fast. Because every single entry in that column is the exact same size in memory, the database engine can calculate and jump to specific rows instantly without having to measure anything.
*   **Cons:** It wastes disk space if the data you put in is much shorter than the maximum length.
*   **Best Use Cases:** Data that is *always* the exact same length. 
    *   Country Codes (`'US'`, `'IN'`) -> `CHAR(2)`
    *   Standardized Phone Numbers
    *   Fixed-length hashes (like an MD5 password hash)
    *   Yes/No or M/F flags -> `CHAR(1)`

## 2. The `VARCHAR` Data Type (Variable-Length)
A `VARCHAR` refers to a variable-length string. `VARCHAR`s also require you to specify the maximum possible length of a string that could be stored in that column, e.g. `VARCHAR(99)`.

### How it works under the hood:
If you define a column as `VARCHAR(50)` and you insert the name `'Ali'`, the database only uses the exact space needed for those 3 characters (plus 1 or 2 extra bytes hidden behind the scenes just to record the length of the string). It does *not* pad the data with extra spaces.

*   **Pros:** It saves a massive amount of disk space. If someone has a short name and another person has a long name, the database only allocates exactly what is needed for each row.
*   **Cons:** It is slightly slower than `CHAR`. The database engine has to read the hidden "length" bytes first to figure out how big the text is before it can process it. It can also cause "fragmentation" if you update a row later with a much longer string.
*   **Best Use Cases:** Almost all standard text where the length is unpredictable.
    *   First and Last Names
    *   Email Addresses
    *   Home Addresses
    *   Blog Post Titles

---
**Key Takeaway:** 
Use `CHAR` for speed when you know exactly how long the text will always be. Use `VARCHAR` for efficiency when the text length will constantly vary!
