# Flask Basics

In this lesson, we will explore the foundational concepts of Flask, a popular micro web framework for Python. You will learn about routing, request handling, templates, and how to serve static files. By the end of this lesson, you will have the skills necessary to create a simple web application using Flask.

## What is Routing?

Routing is the mechanism by which Flask determines how to respond to a client request based on the URL. Each route is associated with a specific function that is executed when the route is accessed.

### Defining Routes

In Flask, routes are defined using the `@app.route()` decorator. Here’s an example of how to define a simple route:

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return 'Welcome to the Flask Basics Lesson!'

if __name__ == '__main__':
    app.run(debug=True)
```

In this example, we defined a route for the root URL (`/`). When a user navigates to this URL, the `home` function is executed, returning a welcome message.

### Dynamic Routing

Flask also supports dynamic routing, allowing you to capture values from the URL. For example:

```python
@app.route('/user/<username>')
def show_user_profile(username):
    return f'User: {username}'
```

In this case, when a user navigates to `/user/john`, the `show_user_profile` function will be called with `username` set to `'john'`.

## Handling HTTP Methods

Flask allows you to handle different HTTP methods such as GET, POST, PUT, and DELETE. By default, routes respond to GET requests. To specify other methods, you can use the `methods` parameter in the route decorator:

```python
@app.route('/submit', methods=['POST'])
def submit_form():
    return 'Form submitted!'
```

In this example, the `submit_form` function will only be called when a POST request is made to the `/submit` route.

## Request and Response Objects

Flask provides request and response objects to handle incoming data and send responses back to the client.

### The Request Object

The `request` object contains all the information about the incoming request, including form data, query parameters, and headers. Here’s an example of how to access form data:

```python
from flask import request

@app.route('/login', methods=['POST'])
def login():
    username = request.form['username']
    password = request.form['password']
    return f'Username: {username}, Password: {password}'
```

### The Response Object

You can return various types of responses from your Flask routes, including strings, HTML, JSON, and more. For example, to return JSON data, you can use the `jsonify` function:

```python
from flask import jsonify

@app.route('/data')
def data():
    return jsonify({'key': 'value'})
```

## Templating with Jinja2

Flask uses the Jinja2 templating engine to render HTML templates. This allows you to separate your application logic from presentation.

### Creating Templates

First, create a folder named `templates` in your project directory. Inside this folder, create an HTML file named `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Flask Basics</title>
  </head>
  <body>
    <h1>Welcome to Flask!</h1>
  </body>
</html>
```

### Rendering Templates

You can render templates in your Flask routes using the `render_template` function:

```python
from flask import render_template

@app.route('/')
def home():
    return render_template('index.html')
```

When a user navigates to the root URL, Flask will render the `index.html` template and return it as the response.

## Serving Static Files

Flask can serve static files such as CSS, JavaScript, and images. By default, Flask looks for static files in a folder named `static`.

### Creating a Static Folder

Create a folder named `static` in your project directory. Inside this folder, you can add your static files. For example, create a CSS file named `style.css`:

```css
body {
  background-color: lightblue;
}
```

### Linking Static Files in Templates

You can link static files in your HTML templates using the `url_for` function:

```html
<link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}" />
```

## Example Application

Now that we have covered the basics, let’s create a simple Flask application that incorporates routing, request handling, templating, and static files.

### Step 1: Create the Application Structure

Create the following directory structure:

```
flask_basics/
│
├── app.py
├── templates/
│   └── index.html
└── static/
    └── style.css
```

### Step 2: Write the Application Code

In `app.py`, add the following code:

```python
from flask import Flask, render_template, request, jsonify

app = Flask(__name__)

@app.route('/')
def home():
    return render_template('index.html')

@app.route('/submit', methods=['POST'])
def submit_form():
    username = request.form['username']
    return f'Hello, {username}!'

if __name__ == '__main__':
    app.run(debug=True)
```

### Step 3: Create the HTML Template

In `templates/index.html`, add the following code:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Flask Basics</title>
    <link
      rel="stylesheet"
      href="{{ url_for('static', filename='style.css') }}"
    />
  </head>
  <body>
    <h1>Welcome to Flask!</h1>
    <form action="/submit" method="post">
      <input type="text" name="username" placeholder="Enter your name" />
      <button type="submit">Submit</button>
    </form>
  </body>
</html>
```

### Step 4: Create the CSS File

In `static/style.css`, add the following code:

```css
body {
  background-color: lightblue;
  text-align: center;
  font-family: Arial, sans-serif;
}
```

### Step 5: Run the Application

In your terminal, navigate to the project directory and run:

```bash
python app.py
```

Open your web browser and navigate to `http://127.0.0.1:5000/`. You should see your Flask application with a form to submit your name. Upon submission, the application will greet you with your name.

## Conclusion

In this lesson, we covered the basics of Flask, including routing, request handling, templating with Jinja2, and serving static files. You learned how to create a simple web application that incorporates these concepts.

[Next: 03_functional_programming](./03_functional_programming.md)
