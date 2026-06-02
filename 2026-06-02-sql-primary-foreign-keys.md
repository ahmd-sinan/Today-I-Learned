# SQL Relationships: Primary & Foreign Keys 

**Date:** 2026-06-02
**Category:** Databases / SQL
**Tags:** #SQL #Databases #PrimaryKey #ForeignKey #RelationalData

Today I learned the most important concept in database design: how to connect different tables together using Keys! This is what makes a Relational Database "relational".

## 1. `PRIMARY KEY` (The Ultimate ID Badge) 
A **Primary Key** is a column that uniquely identifies every single row in a table. 

Think of it like an Aadhar card number or a Student Roll Number. Even if two students are named "Amit Kumar" and are both 20 years old, their Roll Numbers will always be different.

> **Crucial Note:** When you declare a column as a `PRIMARY KEY`, SQL automatically enforces the rules `UNIQUE` and `NOT NULL`. It is impossible to have a duplicate or empty primary key!

**Example:**
```sql
CREATE TABLE users (
    user_id INTEGER PRIMARY KEY,
    username TEXT NOT NULL
);
```

## 2. `FOREIGN KEY` (The Bridge) 
A **Foreign Key** is a column in one table that specifically points to the Primary Key of another table.

This is how we link data without repeating ourselves. If an app has a `users` table and an `orders` table, we don't need to write the user's name, email, and address on every single order. We just put their `user_id` on the order, and the database uses that Foreign Key to look up the rest of the information!

## 3. Putting It All Together: The Code 
Here is how you write the SQL to create two connected tables.

**Table 1: The Parent Table (`users`)**
```SQL
CREATE TABLE users (
    user_id INTEGER PRIMARY KEY,
    name TEXT NOT NULL
);

INSERT INTO users (user_id, name) VALUES (1, 'Abu');
INSERT INTO users (user_id, name) VALUES (2, 'Rahul');
```

**Table 2: The Child Table (`orders`)**
Notice how we create the `user_id` column, and then at the very end, we explicitly tell SQL that it is a `FOREIGN KEY` pointing to the `users` table!
```SQL
CREATE TABLE orders (
    order_id INTEGER PRIMARY KEY,
    item_name TEXT NOT NULL,
    user_id INTEGER,
    FOREIGN KEY(user_id) REFERENCES users(user_id)
);

INSERT INTO orders (order_id, item_name, user_id) VALUES (101, 'Gaming Mouse', 1);
INSERT INTO orders (order_id, item_name, user_id) VALUES (102, 'Mechanical Keyboard', 1);
INSERT INTO orders (order_id, item_name, user_id) VALUES (103, 'Monitor', 2);
```

## 4. Visualizing the Relationship 
Because of the Foreign Key, the database knows exactly who ordered what, without ever having to type the names "Abu" or "Rahul" into the `orders` table!
- **`users` Table:**
| user_id (PRIMARY) | name |
| :--- | :--- |
| 1 | Abu |
| 2 | Rahul |

- **`orders` Table:**
| order_id (PRIMARY) | item_name | user_id (FOREIGN) |
| :--- | :--- | :--- |
| 101 | Gaming Mouse1 | (Points to Abu) |
| 102 | Mechanical Keyboard1 | (Points to Abu) |
| 103 | Monitor2 | (Points to Rahul) |

**Key Takeaway:** Primary Keys ensure every piece of data is uniquely identifiable. Foreign Keys allow tables to share that data instantly. This eliminates duplicate data, saves massive amounts of memory, and keeps the database lightning fast!

