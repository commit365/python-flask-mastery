# SQL Database Integration with Flask

In this lesson, we will explore how to integrate a SQL database with a Flask application. We will cover the basics of setting up a database, connecting to it using SQLAlchemy, performing CRUD (Create, Read, Update, Delete) operations, and handling database migrations. By the end of this lesson, you will have a solid understanding of how to work with SQL databases in your Flask applications.

## What is SQLAlchemy?

SQLAlchemy is a powerful SQL toolkit and Object-Relational Mapping (ORM) library for Python. It provides a set of high-level API for interacting with databases, allowing developers to work with database records as Python objects. SQLAlchemy abstracts the complexities of SQL queries and provides a more Pythonic way to interact with databases.

### Key Features of SQLAlchemy

1. **ORM Capabilities**: SQLAlchemy allows you to define Python classes that map to database tables, enabling you to work with database records as objects.

2. **Database Abstraction**: SQLAlchemy supports multiple database backends, including SQLite, PostgreSQL, MySQL, and more, allowing you to switch databases with minimal changes to your code.

3. **Connection Pooling**: SQLAlchemy manages database connections efficiently, providing connection pooling to improve performance.

4. **Migrations**: SQLAlchemy works well with Alembic, a lightweight database migration tool, making it easy to manage database schema changes.

## Setting Up the Flask Application

### Step 1: Create the Application Structure

Create a new directory for your Flask application:

```
flask_sql_integration/
│
├── app.py
├── models.py
├── requirements.txt
└── config.py
```

### Step 2: Install Flask and SQLAlchemy

In your terminal, navigate to the `flask_sql_integration` directory and create a `requirements.txt` file with the following content:

```
Flask
Flask-SQLAlchemy
Flask-Migrate
```

Then, install the dependencies:

```bash
pip install -r requirements.txt
```

### Step 3: Configure the Database Connection

In `config.py`, add the following code to configure the database connection:

```python
import os

class Config:
    SQLALCHEMY_DATABASE_URI = os.getenv('DATABASE_URL', 'sqlite:///site.db')
    SQLALCHEMY_TRACK_MODIFICATIONS = False
```

In this example, we are using SQLite as the database. You can change the `SQLALCHEMY_DATABASE_URI` to connect to other databases like PostgreSQL or MySQL by providing the appropriate connection string.

### Step 4: Define the Database Models

In `models.py`, define the database models using SQLAlchemy:

```python
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()

class Task(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(100), nullable=False)
    description = db.Column(db.String(200), nullable=True)

    def __repr__(self):
        return f"<Task {self.title}>"
```

In this example, we defined a `Task` model with three fields: `id`, `title`, and `description`.

### Step 5: Set Up the Flask Application

In `app.py`, set up the Flask application and initialize the database:

```python
from flask import Flask, jsonify, request
from models import db, Task
from config import Config

app = Flask(__name__)
app.config.from_object(Config)
db.init_app(app)

with app.app_context():
    db.create_all()  # Create database tables

@app.route('/tasks', methods=['POST'])
def create_task():
    data = request.get_json()
    new_task = Task(title=data['title'], description=data.get('description'))
    db.session.add(new_task)
    db.session.commit()
    return jsonify({'id': new_task.id, 'title': new_task.title, 'description': new_task.description}), 201

@app.route('/tasks', methods=['GET'])
def get_tasks():
    tasks = Task.query.all()
    return jsonify([{'id': task.id, 'title': task.title, 'description': task.description} for task in tasks])

@app.route('/tasks/<int:task_id>', methods=['GET'])
def get_task(task_id):
    task = Task.query.get_or_404(task_id)
    return jsonify({'id': task.id, 'title': task.title, 'description': task.description})

@app.route('/tasks/<int:task_id>', methods=['PUT'])
def update_task(task_id):
    task = Task.query.get_or_404(task_id)
    data = request.get_json()
    task.title = data.get('title', task.title)
    task.description = data.get('description', task.description)
    db.session.commit()
    return jsonify({'id': task.id, 'title': task.title, 'description': task.description})

@app.route('/tasks/<int:task_id>', methods=['DELETE'])
def delete_task(task_id):
    task = Task.query.get_or_404(task_id)
    db.session.delete(task)
    db.session.commit()
    return jsonify({'message': 'Task deleted'}), 204

if __name__ == '__main__':
    app.run(debug=True)
```

### Step 6: Run the Application

In your terminal, navigate to the `flask_sql_integration` directory and run:

```bash
python app.py
```

### Step 7: Testing the API Endpoints

You can use a tool like Postman or cURL to test the API endpoints. Here are the available endpoints:

1. **Create a New Task**:

   - **Endpoint**: `POST /tasks`
   - **Description**: Create a new task.
   - **Example Request**:
     ```bash
     curl -X POST http://127.0.0.1:5000/tasks -H "Content-Type: application/json" -d '{"title": "Task 1", "description": "Description for task 1"}'
     ```

2. **Get All Tasks**:

   - **Endpoint**: `GET /tasks`
   - **Description**: Retrieve a list of all tasks.
   - **Example Request**:
     ```bash
     curl -X GET http://127.0.0.1:5000/tasks
     ```

3. **Get a Specific Task**:

   - **Endpoint**: `GET /tasks/<task_id>`
   - **Description**: Retrieve a specific task by its ID.
   - **Example Request**:
     ```bash
     curl -X GET http://127.0.0.1:5000/tasks/1
     ```

4. **Update a Task**:

   - **Endpoint**: `PUT /tasks/<task_id>`
   - **Description**: Update an existing task by its ID.
   - **Example Request**:
     ```bash
     curl -X PUT http://127.0.0.1:5000/tasks/1 -H "Content-Type: application/json" -d '{"title": "Updated Task 1", "description": "Updated description for task 1"}'
     ```

5. **Delete a Task**:
   - **Endpoint**: `DELETE /tasks/<task_id>`
   - **Description**: Delete a specific task by its ID.
   - **Example Request**:
     ```bash
     curl -X DELETE http://127.0.0.1:5000/tasks/1
     ```

## Handling Database Migrations

To manage changes to the database schema over time, you can use Flask-Migrate, which is built on top of Alembic. Flask-Migrate allows you to handle database migrations easily.

### Step 1: Initialize Flask-Migrate

Add Flask-Migrate to your `requirements.txt`:

```
Flask-Migrate
```

Then, initialize Flask-Migrate in your application:

```python
from flask_migrate import Migrate

migrate = Migrate(app, db)
```

### Step 2: Create Migration Scripts

To create a migration script, run the following command in your terminal:

```bash
flask db init  # Initialize the migration repository
flask db migrate -m "Initial migration."  # Create a migration script
flask db upgrade  # Apply the migration to the database
```

### Step 3: Updating the Database Schema

When you make changes to your models, you can create a new migration script:

```bash
flask db migrate -m "Add new field to Task"
flask db upgrade  # Apply the new migration
```

## Conclusion

In this lesson, we learned how to integrate a SQL database with a Flask application using SQLAlchemy. We covered how to set up a database connection, define models, perform CRUD operations, and manage database migrations with Flask-Migrate.

This foundational knowledge will enable you to create robust applications that interact with databases effectively.

[Next: 08_firebase_integration](./08_firebase_integration.md)
