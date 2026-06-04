# Database Data Types: SQLite vs. MySQL 

**Date:** 2026-06-04

**Category:** Databases / SQL

**Tags:** #SQL #Databases #DataTypes #MySQL #SQLite

Today's lesson highlights the stark contrast between how different database engines handle data types! 

## 1. SQLite: The 5 Core Data Types (Dynamic Typing)
SQLite keeps things incredibly simple by categorizing data into just five core storage classes. When creating a column in SQL, you assign it a "type" so the computer knows exactly how to store it in memory.

*   **INTEGER:** Used for whole numbers. Example: Age, Quantities, or ID numbers (`20`, `500`, `-10`).
*   **TEXT:** Used for strings and characters. Example: Names, Emails, or Passwords (`'Abu'`, `'hello@email.com'`).
*   **REAL:** Used for floating-point numbers (decimals). Example: Prices, GPA, or exact measurements (`99.99`, `3.14`).
*   **NUMERIC / NUMERICAL:** Often used to store values exactly as they are. In many SQL databases, this is used for strict formats like Dates and Times, or boolean true/false values. Example: Dates (`'2026-06-01'`) or exact currency.
*   **BLOB (Binary Large Objects):** Used to store raw binary data (0s and 1s) exactly as it was input. Example: You use this when you want to save actual files directly into the database, like an uploaded profile picture (`.jpg`), an audio file (`.mp3`), or a PDF.

## 2. MySQL / Standard SQL: The Granular Approach (Strict Typing)
Unlike SQLite, engines like MySQL offer a massive grid of highly specific data types to optimize memory and performance. Here are the 20 distinct types shown in the standard SQL breakdown:

*   **Integers:** `INT`, `SMALLINT`, `TINYINT`, `MEDIUMINT`, and `BIGINT`. 
*   **Decimals:** `DECIMAL` and `FLOAT`.
*   **Dates & Times:** `DATE`, `TIME`, `DATETIME`, and `TIMESTAMP`.
*   **Strings & Text:** `CHAR`, `VARCHAR`, and `TEXT`.
*   **Binary Data:** `BIT`, `BINARY`, and `BLOB`.
*   **Specialized Types:** `ENUM`, `GEOMETRY`, and `LINESTRING`.