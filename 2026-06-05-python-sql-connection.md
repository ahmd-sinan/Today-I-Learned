# Connecting Python to SQL

**Date:** 2026-06-04
**Category:** Python / Databases
**Tags:** #Python #SQL #SQLite3 #Backend #FullStack

Today I learned how to connect Python directly to a database using the industry-standard `sqlite3` module. It is built right into Python, so I don't even need to use `pip install`!

## 1. The Core Concepts 
To talk to a database in pure Python, you always follow a specific 4-step sequence:
1.  **Connect:** Open the `.db` file.
2.  **Cursor:** Create a "cursor" (a pointer that travels into the database to execute commands and grab data).
3.  **Execute & Fetch/Commit:** Run the SQL command. If you are reading data, you `fetch` it. If you are writing data, you `commit` it.
4.  **Close:** Shut the connection to free up system memory.

## 2. Reading Data (`SELECT`) 
When you run a `SELECT` query, Python needs to bring that data back into your script. You use `fetchall()` to get a list of all rows, or `fetchone()` to get just the first row.

```python
import sqlite3

# 1. Open the connection to the database file
conn = sqlite3.connect("students.db")

# 2. Create the cursor pointer
cursor = conn.cursor()

# 3. Execute the safe, parameterized SQL query
target_age = 20
cursor.execute("SELECT Name, Age FROM sheet1 WHERE Age = ?", (target_age,))

# 4. Fetch the results (returns a list of tuples)
students = cursor.fetchall()

for student in students:
    print(f"Name: {student[0]}, Age: {student[1]}")

# 5. Close the connection
conn.close()
```

Notice the (target_age,)` syntax! When passing a single parameter in Python `sqlite3`, it MUST be a tuple, so you have to leave that trailing comma!

## 3. Writing Data (`INSERT`, `UPDATE`, `DELETE`) 
When you change data in the database, `execute()` alone doesn't actually save it permanently. It just queues up the changes. You MUST call `conn.commit() to finalize the write!

```python
import sqlite3

conn = sqlite3.connect("students.db")
cursor = conn.cursor()

# Get user input safely
new_id = int(input("ID: "))
new_name = input("Name: ")
new_age = int(input("Age: "))

# Execute the INSERT statement using placeholders to prevent SQL Injection
cursor.execute("INSERT INTO sheet1 (ID, Name, Age) VALUES (?, ?, ?)", (new_id, new_name, new_age))

# Save the changes to the hard drive!
conn.commit()
print(f"Successfully added {new_name} to the database!")

conn.close()
```

## 4. Context Managers (The Pro Way) 
If your program crashes before it reaches `conn.close()`, the database might stay locked in memory!

To write professional, bulletproof Python, developers use the `with` keyword (a Context Manager) combined with a `try/except` block.
- It automatically commits changes if the code succeeds.
- It automatically rolls back changes if an error occurs.
- It automatically closes the connection when finished.

```Python
import sqlite3

# The 'with' statement handles opening and closing automatically!
with sqlite3.connect("students.db") as conn:
    cursor = conn.cursor()
    cursor.execute("SELECT COUNT(*) FROM sheet1")
    
    # fetchone() grabs the single row, and [0] grabs the first column (the count)
    total_users = cursor.fetchone()[0] 
    print(f"Total students in DB: {total_users}")
```
**Another Example:**

```python
import sqlite3

# Wrap the whole database operation in a try-except block to catch crashes
try:
    # The 'with' statement handles opening, closing, and committing automatically!
    with sqlite3.connect("students.db") as conn:
        cursor = conn.cursor()
        
        # 1. Let's try to write some new data
        new_student = (12, "Zayn", 20)
        cursor.execute("INSERT INTO sheet1 (ID, Name, Age) VALUES (?, ?, ?)", new_student)
        
        # 2. Let's read the data back to prove it worked
        cursor.execute("SELECT Name, Age FROM sheet1 ORDER BY Age DESC LIMIT 3")
        top_students = cursor.fetchall()
        
        print("Top 3 Oldest Students:")
        for student in top_students:
            print(f"- {student[0]} (Age: {student[1]})")
            
        # Notice: We DO NOT need to write conn.commit() or conn.close() here!

except sqlite3.IntegrityError:
    # This catches SQL Constraint errors (like a duplicate UNIQUE ID or NOT NULL violation)
    print("Database Error: That ID already exists or a rule was violated!")
    
except Exception as error:
    # This catches any other random Python crashes
    print(f"A fatal error occurred: {error}")
```

## 5. Connecting Python to MySQL (The Client-Server Way) 
Unlike SQLite, MySQL runs as an independent background server. To connect Python to it, we need to download a third-party driver using `pip`. The official one from Oracle is called `mysql-connector-python`

**Step 1: Install the Driver**

Open your terminal and run:

```Bash
$ pip install mysql-connector-python
```

**Step 2: The Connection Code**

When connecting to MySQL, you don't just point to a file. You have to provide the server address (host), the username, the password, and the database name.

(Note: If you are using XAMPP for local development, the host is always `'localhost'`, the default user is `'root'`, and the password is completely blank!)

```python
import mysql.connector

# 1. Connect to the MySQL Server running in the background (like XAMPP)
try:
    conn = mysql.connector.connect(
        host="localhost",
        user="root",
        password="",          # Default XAMPP password is empty
        database="bca_db"     # The name of the database you made in phpMyAdmin
    )

    if conn.is_connected():
        print("Successfully connected to the MySQL server! 🚀")

        # 2. Create the cursor pointer
        cursor = conn.cursor()

        # 3. Execute a secure query using the %s placeholder (MySQL uses %s, not ?)
        target_name = "Abu"
        cursor.execute("SELECT email FROM users WHERE name = %s", (target_name,))

        # 4. Fetch and print the result
        result = cursor.fetchone()
        if result:
            print(f"User Email: {result[0]}")

except mysql.connector.Error as err:
    print(f"Error: {err}")

finally:
    # 5. Always close the connection safely!
    if 'conn' in locals() and conn.is_connected():
        cursor.close()
        conn.close()
        print("MySQL connection closed.")
```

### The Big Difference: Placeholders (`?` vs `%s`) 
This is a massive trap for developers switching between the two engines:

When using Python with SQLite3, the secure placeholder is a question mark: `WHERE Age = ?`

When using Python with MySQL, the secure placeholder is a percent-s: `WHERE Age = %s`

**Key Takeaway:** The overall flow (Connect -> Cursor -> Execute -> Fetch/Commit -> Close) is identical for both SQLite and MySQL! The only difference is that MySQL requires a downloaded driver, network credentials, and the `%s` placeholder!