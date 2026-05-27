# SQL & SQLite3: Mastering Databases 

**Date:** 2026-05-27

Today I entered the world of databases. Instead of saving data into basic `.txt` files, I learned how to use **SQL (Structured Query Language)** to store, search, and manipulate massive amounts of data instantly using **SQLite3**.

## 1. What is SQLite3? 
SQLite3 is a lightweight, file-based database engine that runs directly in the terminal. 
* A **Database (`.db`)** is the main file (like an Excel workbook).
* A **Table** (often thought of as a "sheet") is a grid of rows and columns inside the database.

### The Setup: Importing a CSV
Imagine I have a basic text file called `people.csv` with 10 rows:
```csv
ID,Name,Age
1,Amit,20
2,Priya,19
3,Rahul,21
4,Sneha,20
5,Raju,22
6,Kiran,19
7,Deepa,23
8,Arjun,20
9,Neha,21
10,Vikram,22
```

### The Terminal Commands (`.dot` commands)
To get this into a database, I use SQLite's special `.dot` commands in the terminal:
```bash
# 1. Create/Open a database file
$ sqlite3 students.db

# 2. Tell SQLite we are working with comma-separated values (for the import)
sqlite> .mode csv

# 3. Import the CSV file into a new table (sheet) called 'sheet1'
sqlite> .import people.csv sheet1

# 4. Change the visual output mode to 'box' so it looks like a real table!
sqlite> .mode box

# 5. Read the whole data to verify it imported correctly
sqlite> SELECT * FROM sheet1;
┌────┬────────┬─────┐
│ ID │  Name  │ Age │
├────┼────────┼─────┤
│ 1  │ Amit   │ 20  │
│ 2  │ Priya  │ 19  │
│ 3  │ Rahul  │ 21  │
│ 4  │ Sneha  │ 20  │
│ 5  │ Raju   │ 22  │
│ 6  │ Kiran  │ 19  │
│ 7  │ Deepa  │ 23  │
│ 8  │ Arjun  │ 20  │
│ 9  │ Neha   │ 21  │
│ 10 │ Vikram │ 22  │
└────┴────────┴─────┘

# 6. Safely exit the database engine
sqlite> .quit
```

## 2. The Core SQL Keywords (CRUD) 
SQL revolves around four main operations: Create, Read, Update, Delete. (Note: SQL commands always end with a semicolon `;`)

### A. `SELECT` (Read)
Used to fetch specific data from a table.

```SQL
sqlite> SELECT Age FROM sheet1;
20
19
21
...
```

### B. `INSERT INTO` (Create)
Used to add a brand new row to the table.
```SQL
sqlite> INSERT INTO sheet1 (ID, Name, Age) VALUES (11, 'Abu', 19);
```

### C. `UPDATE` (Update)
Used to change existing data. You MUST use `WHERE` or you will accidentally update every single row!
```SQL
sqlite> UPDATE sheet1 SET Age = 20 WHERE Name = 'Abu';
```

### D. `DELETE` (Delete)
Used to remove a specific row. Again, `WHERE` is crucial!
```SQL
sqlite> DELETE FROM sheet1 WHERE ID = 10;
```

## 3. Filtering Data (`WHERE` & More) 
You rarely want to see all the data. You want specific filters.

`WHERE`: Filters results based on a condition.
```SQL
sqlite> .mode box
sqlite> SELECT * FROM sheet1 WHERE Name = 'Raju';
┌────┬────────┬─────┐
│ ID │  Name  │ Age │
├────┼────────┼─────┤
│ 5  │ Raju   │ 22  │
└────┴────────┴─────┘
```

`AND` / `OR`: Combine multiple conditions.
```SQL
sqlite> SELECT Name FROM sheet1 WHERE Age = 20 AND ID < 5;
Amit
Sneha
```

`ORDER BY`: Sorts the output.
```SQL
sqlite> SELECT * FROM sheet1 ORDER BY Age DESC;
# Returns the table sorted from oldest to youngest!
```

## 4. Aggregate Functions (Math on the Fly!) 
SQL can do math across thousands of rows instantly without needing a Python loop!

`COUNT`: Counts the number of rows.
```SQL
sqlite> SELECT COUNT(*) FROM sheet1;
10
```

`AVG`: Calculates the average of a numerical column.
```SQL
sqlite> SELECT AVG(Age) FROM sheet1;
20.7
```

`MAX` and `MIN`: Finds the highest or lowest value.
```SQL
sqlite> SELECT MAX(Age) FROM sheet1;
23
```
## 5. Expanding the Database (`CREATE` & `DROP`) 
A database can hold many tables (sheets) at once!

**To create a second sheet manually:**
We use `CREATE TABLE` and define the column names and their data types (like `INTEGER` for ints, and `TEXT` for strings).

```SQL
sqlite> CREATE TABLE sheet2 (ID INTEGER, Course TEXT);
sqlite> INSERT INTO sheet2 (ID, Course) VALUES (1, 'BCA');
sqlite> SELECT * FROM sheet2;
1,BCA
```
**To destroy a table (`DROP`):**
If you mess up and want to completely delete an entire table (sheet) from existence, use `DROP`. Be careful, there is no undo!

```SQL
sqlite> DROP TABLE sheet2;
```
**Key Takeaway:** SQL commands are almost like speaking English. `SELECT [what] FROM [table] WHERE [condition]`. By keeping databases separated into files like `students.db`, I can use Python later to run these exact SQL queries directly inside my code!

