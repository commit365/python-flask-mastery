# Introduction to Flask

Welcome to the first lesson of the **Python Flask Mastery Course**! In this lesson, we will introduce you to Flask, a powerful and flexible web framework for Python. By the end of this lesson, you will have a solid understanding of what Flask is, its core features, and how to set up your development environment to start building web applications.

## What is Flask?

Flask is a lightweight web framework for Python that allows developers to build web applications quickly and efficiently. It is classified as a micro-framework because it provides the essential tools needed to build web applications without the overhead of larger frameworks. Flask is designed to be simple and easy to use, making it an excellent choice for both beginners and experienced developers.

### Key Features of Flask

1. **Lightweight and Modular**: Flask is designed to be minimalistic, allowing you to pick and choose the components you need for your application. This modularity makes it easy to extend and customize.

2. **Built-in Development Server**: Flask comes with a built-in development server that allows you to test your applications locally without the need for additional configuration.

3. **Routing**: Flask provides a simple and intuitive way to define routes for your application, allowing you to map URLs to Python functions.

4. **Templating**: Flask uses Jinja2 as its templating engine, enabling you to create dynamic HTML pages by combining HTML with Python code.

5. **RESTful Request Dispatching**: Flask makes it easy to build RESTful APIs, allowing you to create web services that can be consumed by front-end applications.

6. **Support for Extensions**: Flask has a rich ecosystem of extensions that add functionality to your application, such as database integration, authentication, and form handling.

7. **Community and Documentation**: Flask has a large and active community, providing extensive documentation, tutorials, and third-party libraries.

## Why Use Flask?

Flask is an excellent choice for web development for several reasons:

- **Ease of Use**: Flask's simplicity makes it easy to learn, especially for those new to web development.
- **Flexibility**: You can structure your application as you see fit, without being constrained by a specific framework architecture.
- **Performance**: Flask is lightweight and fast, making it suitable for small to medium-sized applications.
- **Scalability**: While Flask is simple, it can be scaled up to handle larger applications by integrating with various extensions and libraries.

## Setting Up Your Development Environment

To start building applications with Flask, you need to set up your development environment. Follow these steps to get started:

### Prerequisites

- **Python**: Ensure you have Python 3.6 or higher installed on your machine. You can check your Python version by running:

  ```bash
  python3 --version
  ```

- **pip**: This is the package manager for Python. It is usually included with Python installations. You can check if pip is installed by running:
  ```bash
  pip3 --version
  ```

### Step 1: Create a Virtual Environment

It is a good practice to create a virtual environment for your Flask projects to manage dependencies. Run the following commands in your terminal:

```bash
# Navigate to your desired project directory
cd path/to/your/project-directory

# Create a virtual environment named 'venv'
python3 -m venv venv

# Activate the virtual environment
source venv/bin/activate
```

### Step 2: Install Flask

Once your virtual environment is activated, you can install Flask using pip:

```bash
pip install Flask
```

### Step 3: Verify the Installation

To verify that Flask is installed correctly, you can run the following command in your terminal:

```bash
python -m flask --version
```

You should see the Flask version number, confirming that the installation was successful.

## Creating Your First Flask Application

Now that you have set up your environment and installed Flask, let’s create a simple Flask application.

### Step 1: Create a New File

Create a new file named `app.py` in your project directory:

```bash
touch app.py
```

### Step 2: Write Your Flask Application

Open `app.py` in your favorite text editor and add the following code:

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello, World!'

if __name__ == '__main__':
    app.run(debug=True)
```

### Step 3: Run Your Application

In your terminal, run the following command to start the Flask development server:

```bash
python app.py
```

You should see output indicating that the server is running. Open your web browser and navigate to `http://127.0.0.1:5000/`. You should see the message "Hello, World!" displayed on the page.

### Step 4: Stop the Server

To stop the server, you can press `Ctrl + C` in the terminal where the server is running.

## Conclusion

In this lesson, we introduced you to Flask, a powerful micro-framework for building web applications in Python. You learned about its key features, the reasons to use Flask, and how to set up your development environment. Finally, you created your first Flask application that returns a simple "Hello, World!" message.

[Next: 02_flask_basics](./02_flask_basics.md)
