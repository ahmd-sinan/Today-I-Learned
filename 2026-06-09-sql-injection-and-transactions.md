# Database Security: SQL Injection & Transactions 

**Date:** 2026-06-09

**Category:** Databases / Security
**Tags:** #SQL #Security #SQLInjection #Transactions #Python

Today I learned how to securely write SQL queries inside Python. If you just recklessly jam user input into a SQL string, a hacker can easily destroy your database or bypass your login screens!

## 1. The Threat: SQL Injection 
**SQL Injection (SQLi)** is a cyberattack where a malicious user types actual SQL commands into a normal input field (like a search bar or username box) to trick the database into executing their code instead of your code.

### The Mistake (Using f-strings):
If you use Python's f-strings to build your SQL query, you are directly pasting whatever the user typed right into the engine.

```python
# DANGEROUS CODE
favorite = input("Favorite: ") 

# If the user types:   ' OR '1'='1
# The f-string becomes: SELECT COUNT(*) FROM favorites WHERE problem = '' OR '1'='1'
rows = db.execute(f"SELECT COUNT(*) AS n FROM favorites WHERE problem = '{favorite}'")
```

Because `1=1` is always true, the hacker just bypassed your `WHERE` clause completely! They could even type `'; DROP TABLE users; --` and delete your entire database!

## 2. The Solution: Parameterized Queries (Placeholders) 
To fix this, you must **NEVER** trust user input. Instead of f-strings, you use a placeholder `?`  in your SQL string, and pass the variable as a separate argument.

```Python
# SAFE CODE
favorite = input("Favorite: ")

# Notice the '?' and how the variable is passed separately!
rows = db.execute("SELECT COUNT(*) AS n FROM favorites WHERE problem = ?", favorite)
```
**Why this works:** When you use the `?` placeholder, the database library takes the user's input and completely "sanitizes" it. It treats whatever the user typed strictly as raw text data, completely stripping it of any executable SQL powers.

## 3. Race Conditions & Transactions 
SQL Injection isn't the only danger. You also have to protect your data from **Race Conditions**.

Imagine an Instagram post with 100 likes. Two different users hit the "Like" button at the exact same millisecond.

1. Server A reads the likes: 100.

2. Server B reads the likes: 100.

3. Server A adds 1 and updates the database to 101.

4. Server B adds 1 and updates the database to 101.
Result: We lost a like! It should be 102.

**The Fix:** `BEGIN TRANSACTION` and `COMMIT` 
To solve this, we can wrap our SQL commands in a "Transaction". This puts a temporary lock on that specific row of data.

```Python
# 1. Lock the database so no one else can touch it
db.execute("BEGIN TRANSACTION")

# 2. Safely read the current likes
rows = db.execute("SELECT likes FROM posts WHERE id = ?", id)
likes = rows[0]["likes"]

# 3. Update the likes safely
db.execute("UPDATE posts SET likes = ? WHERE id = ?", likes + 1, id)

# 4. Unlock the database and save the changes
db.execute("COMMIT")
```
By placing `BEGIN TRANSACTION` before the `SELECT` and `COMMIT` after the `UPDATE`, we guarantee that if two users click like at the same time, the database forces one of them to wait in line until the first transaction finishes!