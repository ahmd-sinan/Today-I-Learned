# Flask & SQL: Data Persistence and State Management 

**Date:** 2026-07-16
**Category:** Web Development / Backend Engineering
**Tags:** #Python #Flask #SQL #Database #Backend #CRUD

Today I integrated a relational database (SQLite) into my Flask backend. This bridges the gap between the web application (the logic) and persistent storage (the memory). 

## 1. The Persistence Problem (Statelessness) 
Previously, user registrations were stored in a standard Python dictionary. 
* **The Flaw:** RAM is volatile. If the Flask server process restarts, crashes, or is deployed to a new cloud instance, the dictionary is wiped and all user data is permanently lost.
* **The Industry Solution:** Web servers should be inherently *stateless*. State (user data) must be offloaded to a persistent SQL database (like SQLite, PostgreSQL, or MySQL) living on a hard drive. 

## 2. Reading & Writing to SQL via Python 
To connect Flask to a database, backend developers execute standard SQL queries directly from their Python routing functions.

### Writing Data (`INSERT`)
When a user submits a registration form via `POST`, the backend captures the data and inserts it into a SQL table.

```python
from flask import Flask, redirect, render_template, request
# Assuming a standard Python SQL connector setup here
app = Flask(__name__)

@app.route("/register", methods=["POST"])
def register():
    name = request.form.get("name")
    # Handling checkboxes (arrays of data) instead of single inputs
    sports = request.form.getlist("sport") 
    
    # Validation logic omitted for brevity...

    # Database Execution
    for sport in sports:
        db.execute("INSERT INTO registrants (name, sport) VALUES(?, ?)", name, sport)

    return redirect("/registrants")
```

## 3. Jinja2 Logic: Handling Missing Data 
When building templates, you cannot assume the user provided valid data. Jinja2 allows you to write conditional logic (`if/else`) directly into the HTML to handle edge cases.

**Template (`greet.html`):**

```HTML
{% extends "layout.html" %}
{% block body %}
    <h1>hello, 
    {% if name %}
        {{ name }}
    {% else %}
        world
    {% endif %}!
    </h1>
{% endblock %}
```
**Note:** The dash syntax (`{%- if -%}`) can be used to strip out extra whitespace rendered by the template engine, keeping the final HTML clean.

## 4. Server-Side Validation 
**The Concept:** Never trust user input. Even if you restrict an HTML form (e.g., using a dropdown), a malicious user can easily use "Inspect Element" to modify the HTML and submit fake data. Therefore, you must always validate the data on the server side (in Python) before processing it.

```Python
SPORTS = ["Basketball", "Soccer", "Ultimate Frisbee"]

@app.route("/register", methods=["POST"])
def register():
    # 1. Validate that the name field wasn't submitted empty
    name = request.form.get("name")
    if not name:
        return render_template("error.html", message="Missing name")

    # 2. Validate that the sport exists AND that it hasn't been maliciously altered
    sport = request.form.get("sport")
    if sport not in SPORTS:
        return render_template("error.html", message="Invalid sport selection")

    # 3. If validation passes, process the registration
    REGISTRANTS[name] = sport
    return redirect("/registrants")
```

## 5. State Management & The In-Memory Flaw 
In the example above, user registrations are stored in a simple Python dictionary (`REGISTRANTS = {}`).

- **The Architectural Flaw:**
While this works for a quick test, it is entirely "in-memory." If the Flask server process restarts, crashes, or shuts down, the dictionary is wiped clean and all data is permanently lost.

- **The Solution:**
To make data persist across server restarts, the backend must be connected to a dedicated, persistent storage solution like a Relational Database (MySQL, PostgreSQL) or a NoSQL Database (MongoDB).