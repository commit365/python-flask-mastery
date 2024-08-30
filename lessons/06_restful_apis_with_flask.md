# Building RESTful APIs with Flask

In this lesson, we will explore how to build RESTful APIs using Flask, a lightweight web framework for Python. REST (Representational State Transfer) is an architectural style that defines a set of constraints for creating web services. We will cover the principles of RESTful design, how to create endpoints, handle requests and responses, and implement CRUD (Create, Read, Update, Delete) operations.

## What is a RESTful API?

A RESTful API is an application programming interface that adheres to the principles of REST. It allows clients to interact with a server using standard HTTP methods such as GET, POST, PUT, and DELETE. RESTful APIs are stateless, meaning that each request from a client contains all the information needed to process that request.

### Key Principles of RESTful APIs

1. **Statelessness**: Each request from a client must contain all the information needed to understand and process the request. The server does not store any client context between requests.

2. **Resource-Based**: RESTful APIs are centered around resources, which are identified by URIs (Uniform Resource Identifiers). Resources can be represented in various formats, such as JSON or XML.

3. **HTTP Methods**: RESTful APIs use standard HTTP methods to perform operations on resources:

   - **GET**: Retrieve a resource or a collection of resources.
   - **POST**: Create a new resource.
   - **PUT**: Update an existing resource.
   - **DELETE**: Remove a resource.

4. **Stateless Communication**: Each request is independent, and the server does not maintain any session state.

## Setting Up a Flask Application

### Step 1: Create the Application Structure

Create a new directory for your Flask RESTful API:

```
flask_rest_api/
│
├── app.py
└── requirements.txt
```

### Step 2: Install Flask and Flask-RESTful

In your terminal, navigate to the `flask_rest_api` directory and create a `requirements.txt` file with the following content:

```
Flask
Flask-RESTful
```

Then, install the dependencies:

```bash
pip install -r requirements.txt
```

### Step 3: Write the Application Code

In `app.py`, add the following code to set up a basic Flask application with Flask-RESTful:

```python
from flask import Flask, jsonify, request
from flask_restful import Api, Resource

app = Flask(__name__)
api = Api(app)

# Sample data to simulate a database
tasks = [
    {'id': 1, 'title': 'Task 1', 'description': 'Description for task 1'},
    {'id': 2, 'title': 'Task 2', 'description': 'Description for task 2'}
]

class Task(Resource):
    def get(self, task_id):
        task = next((task for task in tasks if task['id'] == task_id), None)
        if task:
            return jsonify(task)
        return {'message': 'Task not found'}, 404

    def delete(self, task_id):
        global tasks
        tasks = [task for task in tasks if task['id'] != task_id]
        return {'message': 'Task deleted'}, 204

    def put(self, task_id):
        task = next((task for task in tasks if task['id'] == task_id), None)
        if not task:
            return {'message': 'Task not found'}, 404

        data = request.get_json()
        task['title'] = data.get('title', task['title'])
        task['description'] = data.get('description', task['description'])
        return jsonify(task)

class TaskList(Resource):
    def get(self):
        return jsonify(tasks)

    def post(self):
        data = request.get_json()
        new_task = {
            'id': len(tasks) + 1,
            'title': data['title'],
            'description': data['description']
        }
        tasks.append(new_task)
        return jsonify(new_task), 201

# Define the API endpoints
api.add_resource(TaskList, '/tasks')
api.add_resource(Task, '/tasks/<int:task_id>')

if __name__ == '__main__':
    app.run(debug=True)
```

### Step 4: Run the Application

In your terminal, navigate to the `flask_rest_api` directory and run:

```bash
python app.py
```

### Step 5: Testing the API Endpoints

You can use a tool like Postman or cURL to test the API endpoints. Here are the available endpoints:

1. **Get All Tasks**:

   - **Endpoint**: `GET /tasks`
   - **Description**: Retrieve a list of all tasks.
   - **Example Request**:
     ```bash
     curl -X GET http://127.0.0.1:5000/tasks
     ```

2. **Get a Specific Task**:

   - **Endpoint**: `GET /tasks/<task_id>`
   - **Description**: Retrieve a specific task by its ID.
   - **Example Request**:
     ```bash
     curl -X GET http://127.0.0.1:5000/tasks/1
     ```

3. **Create a New Task**:

   - **Endpoint**: `POST /tasks`
   - **Description**: Create a new task.
   - **Example Request**:
     ```bash
     curl -X POST http://127.0.0.1:5000/tasks -H "Content-Type: application/json" -d '{"title": "Task 3", "description": "Description for task 3"}'
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

## Error Handling

When building RESTful APIs, it is important to handle errors gracefully. You can create custom error responses for different scenarios. For example, you can modify the `get` method in the `Task` class to return a custom error message when a task is not found:

```python
def get(self, task_id):
    task = next((task for task in tasks if task['id'] == task_id), None)
    if task:
        return jsonify(task)
    return {'message': 'Task not found'}, 404
```

### Handling Invalid JSON

You should also handle cases where the client sends invalid JSON data. You can do this by checking if the request data is valid before processing it:

```python
def post(self):
    data = request.get_json()
    if not data or 'title' not in data or 'description' not in data:
        return {'message': 'Invalid input'}, 400
    # Proceed with creating the task
```

## Conclusion

In this lesson, we learned how to build RESTful APIs using Flask. We covered the principles of RESTful design, created endpoints for CRUD operations, and implemented error handling. By following RESTful conventions, you can create APIs that are easy to use and understand.

[Next: 07_sql_database_integration](./07_sql_database_integration.md)
