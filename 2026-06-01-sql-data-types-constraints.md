# SQL Architecture: Data Types & Constraints 

**Date:** 2026-06-01

Today I learned how to strictly define the rules of my database tables. By setting specific **Data Types** and **Constraints**, I can protect my database from bad data and keep everything running efficiently!

## 1. The 5 Core Data Types 
When creating a column in SQL, you assign it a "type" so the computer knows exactly how to store it in memory.

*   **INTEGER:** Used for whole numbers.
    *   *Example:* Age, Quantities, or ID numbers (`20`, `500`, `-10`).
*   **TEXT:** Used for strings and characters.
    *   *Example:* Names, Emails, or Passwords (`'Abu'`, `'hello@email.com'`).
*   **REAL:** Used for floating-point numbers (decimals).
    *   *Example:* Prices, GPA, or exact measurements (`99.99`, `3.14`).
*   **NUMERIC / NUMERICAL:** Often used to store values exactly as they are. In many SQL databases, this is used for strict formats like Dates and Times, or boolean true/false values.
    *   *Example:* Dates (`'2026-06-01'`) or exact currency.
*   **BLOB (Binary Large Objects):** Used to store raw binary data (0s and 1s) exactly as it was input. 
    *   *Example:* You use this when you want to save actual files directly into the database, like an uploaded profile picture (`.jpg`), an audio file (`.mp3`), or a PDF.

## 2. SQL Constraints (The Bouncers of the Database) 
Data types dictate *what* a column is, but Constraints dictate the *rules* of that column.

*   **NOT NULL:** This rule means the column CANNOT be left empty. If a user tries to create an account without filling out this required field, the database will reject it and throw an error.
    *   *Why use it?* Every user must have a username. You can't let them leave it blank!
*   **UNIQUE:** This rule means every single entry in this column must be completely different from all the others. No duplicates allowed!
    *   *Why use it?* If two people try to register with the exact same Email Address or Student ID, the database blocks the second person.

## 3. Putting It All Together: The `CREATE` Statement 
Here is an example of how a professional database engineer combines all these concepts to create a highly secure `users` table:

```sql
CREATE TABLE users (
    user_id INTEGER UNIQUE NOT NULL,
    username TEXT UNIQUE NOT NULL,
    email TEXT UNIQUE NOT NULL,
    gpa REAL,
    join_date NUMERIC,
    profile_picture BLOB
);
```

**Breaking down the logic of that code:**
- `user_id`: It is a whole number (`INTEGER`), no two users can have the same ID (`UNIQUE`), and it cannot be left blank (`NOT NULL`).
- `username` & `email`: They are text strings (`TEXT`), they must be completely original (`UNIQUE`), and they are mandatory (`NOT NULL`)
- `gpa`: It is a decimal (`REAL`). Notice it doesn't have `NOT NULL`. This means if a user doesn't have a GPA yet, it's perfectly fine to leave this cell empty (NULL)!
- `join_date`: Stored as a date format (`NUMERIC`).
- `profile_picture`: Ready to accept the raw 0s and 1s of an uploaded image file (`BLOB`)