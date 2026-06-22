# Database Engines: MySQL vs. SQLite

**Date:** 2026-05-27
**Category:** Databases / Architecture
**Tags:** #SQL #Databases #MySQL #SQLite #Backend #Architecture

Today I learned about **Database Engines** (storage engines). This is the core software that a Database Management System (DBMS) uses to store, read, update, and delete data. It is the actual "brain" and "filing system" working behind the scenes of a SQL query.

While there are many types of engines, the two below represent the biggest architectural divide in the SQL world: Client-Server databases and Embedded databases.

## 1. Client-Server Database Engines 
**Examples:** MySQL, PostgreSQL

In this model, the database runs as a completely separate background program (a server). When an application wants data, it sends a request over a network connection to that server, which processes it and sends the data back.

### Deep Dive: MySQL
*   **How it works:** It is a heavy-duty, standalone server program designed to handle multiple users and applications connecting at the exact same time over a network
*   **Concurrency:** It uses granular "row-level locking." This means if one user is updating a specific row of data, another user can simultaneously update a different row in the same table without waiting! 🚦
*   **Security:** Extremely robust. You can create different accounts with passwords and fine-grained permissions for who can see or change specific data.
*   **Best Use Cases:** Web applications, eCommerce sites, and content management systems. If your app needs to handle high traffic and many concurrent writers, you need a client-server engine. 

## 2. Embedded Database Engines 
**Examples:** SQLite3, DuckDB

In this model, there is no server process at all. The database is just a library of code (written in C) that gets bundled directly into the application itself.

### Deep Dive: SQLite3
*   **How it works:** The app directly opens a normal file on the hard drive (e.g., `database.db`). The entire database—the schema, tables, indexes, and data—lives inside that single file. 
*   **Concurrency:** It usually locks the entire database file when writing. This means it is incredibly fast for reading data, but only one process can write data at a time.
*   **Zero-Configuration:** There are no user accounts, passwords, or network ports to configure. You just point your code at the file and start working! 
*   **Best Use Cases:** Mobile apps (Android and iOS use it heavily for local storage), desktop applications, embedded systems, and local testing. 📱

---

## Summary: The Ultimate Trade-off

| Feature | Client-Server (e.g., MySQL) | Embedded (e.g., SQLite3) |
| :--- | :--- | :--- |
| **Architecture** | Runs as a separate network server | Bundled directly into the app |
| **Storage** | Managed by the server | A single local `.db` file |
| **Concurrency** | High (Row-level locking) | Low (File-level locking during writes) |
| **Security** | Strict user accounts & permissions | None (Relies on OS file permissions) |
| **Setup** | Requires installation and configuration | Zero configuration (Plug and play) |

**The Golden Rule:** 
Use **MySQL** when your data is shared across a network, needs to handle many simultaneous writers, and requires strict security. 
Use **SQLite3** when your data is local to the application, you want absolute simplicity with zero setup, and it does not need to be accessed by multiple network users at once.
