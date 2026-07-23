# Dynamic Web Architecture: Intro to Python Flask & Templating 

**Date:** 2026-07-09

**Category:** Web Development / Backend Engineering
**Tags:** #Python #Flask #Backend #Jinja2 #WebArchitecture

Today I transitioned from Frontend static design to Backend dynamic architecture. Instead of just serving pre-written HTML files, I learned how to use Python to actively generate HTML on the server-side based on user input and dynamic data.

## Static vs. Dynamic Web Pages 
* **Static Pages (Client-Side):** The HTML file exists perfectly formed on the server. When a browser requests it (e.g., via a basic `http-server`), the server simply hands over the exact file. Everyone who visits the page sees the exact same hardcoded text.
* **Dynamic Pages (Server-Side):** The HTML does not exist as a complete file. When a user requests a URL, a backend language (like Python) executes code, pulls data from a database, and generates a custom HTML response on-the-fly. This is how Instagram shows *your* specific profile instead of a generic one.

## Flask 
![Demo](assets/Flask_logo.png)

Flask is an industry-standard "micro-framework" for Python. It provides the essential tools to build web applications without forcing a massive, complicated structure on the developer (unlike heavier frameworks like Django).

### The Core Files
A basic Flask application requires a specific directory structure:
1.  `app.py`: The core Python script containing the backend logic and routing rules.
2.  `requirements.txt`: A simple text file listing the third-party Python libraries needed to run the app (e.g., it will just contain the word `Flask`).
3.  `/templates/`: A dedicated directory where Flask automatically looks for HTML files.

### Routing with Decorators (`@app.route`)
Flask uses Python decorators to map specific URLs to specific functions.

```python
from flask import Flask

# Initialize the application
app = Flask(__name__)

# The Decorator: Listens for a GET request to the root URL "/"
@app.route("/")
def index():
    return "hello, world"
```

- ***Industry Context:*** Instead of requesting a physical file like `example.com/folder/file.html`, the browser requests a "route" like `example.com/greet`. Flask intercepts this request, runs the `greet()` function, and returns the output.


## Dynamic HTML & The `render_template` Function 
Returning hardcoded HTML strings from a Python function is messy and unmaintainable. Instead, Flask uses the `render_template` function to separate the backend logic from the frontend design.

```python
from flask import Flask, render_template, request
app = Flask(__name__)

@app.route("/")
def index():
    # request.args.get looks for '?name=Abu' in the URL. If missing, defaults to 'world'
    user_name = request.args.get("name", "world")
    
    # Injects the Python variable into the HTML template
    return render_template("index.html", name=user_name)
```

## Processing User Input (Forms) 
Users rarely type arguments directly into the URL bar. Instead, they interact with HTML `<form>` elements.

### The HTML Form (`index.html`):
```html
<form action="/greet" method="get">
    <input name="name" placeholder="Name" type="text">
    <button type="submit">Greet</button>
</form>
```

The Backend Receiver (`app.py`):

```Python
@app.route("/greet")
def greet():
    # Captures the data from the 'name' input field and passes it to the next template
    return render_template("greet.html", name=request.args.get("name", "world"))
```

## Jinja2 Templating & Inheritance 
Flask utilizes a powerful templating engine called Jinja2. It allows you to write Python-like logic directly inside your HTML files using double curly braces `{{ }}` for variables and `{% %}` for control structures.

### The DRY Principle (Don't Repeat Yourself)
In an enterprise application with 50 web pages, you do not want to copy and paste the `<head>`, `<nav>`, and `<footer>` into every single file. If you need to change the logo, you would have to edit 50 files!

Jinja2 solves this using Template Inheritance.

### 1. The Master Blueprint (`layout.html`):

```HTML
<!DOCTYPE html>
<html lang="en">
    <head>
        <title>My Flask App</title>
    </head>
    <body>
        {% block body %}{% endblock %}
    </body>
</html>
```

### 2. The Child Page (`greet.html`):

```HTML
{% extends "layout.html" %}

{% block body %}
    <h1>hello, {{ name }}!</h1>
{% endblock %}
```

**The Result:** The child code is incredibly clean and compact. The backend merges the two files together before sending the final, complete HTML document to the user's browser!
