# Firebase Integration with Flask

In this lesson, we will explore how to integrate Firebase with a Flask application. Firebase is a comprehensive app development platform provided by Google that offers a variety of services, including real-time databases, authentication, cloud storage, and hosting. By integrating Firebase into your Flask application, you can leverage these powerful features to enhance your app's functionality and user experience.

## What is Firebase?

Firebase is a cloud-based platform that provides developers with tools and services to build, improve, and grow their applications. It offers a suite of features, including:

- **Firebase Realtime Database**: A NoSQL cloud database that allows for real-time data synchronization across all connected clients.
- **Cloud Firestore**: A flexible, scalable database for mobile, web, and server development from Firebase and Google Cloud Platform.
- **Firebase Authentication**: A service that simplifies the process of building secure authentication systems.
- **Cloud Functions**: A serverless framework that allows you to run backend code in response to events triggered by Firebase features and HTTPS requests.
- **Firebase Hosting**: A fast and secure way to host static assets and dynamic web applications.

## Setting Up Firebase

### Step 1: Create a Firebase Project

1. Go to the [Firebase Console](https://console.firebase.google.com/).
2. Click on **Add project**.
3. Enter a project name and click **Continue**.
4. (Optional) Enable Google Analytics for your project.
5. Click **Create project** and then **Continue** to access your project dashboard.

### Step 2: Add Firebase to Your Web App

1. In the Firebase Console, select your project.
2. Click on the **Web** icon to add Firebase to your web application.
3. Register your app by providing a nickname and clicking **Register app**.
4. Copy the Firebase configuration object provided, which contains your API key, project ID, and other necessary information.

### Step 3: Install Firebase Admin SDK

To interact with Firebase services from your Flask backend, you will need to install the Firebase Admin SDK. You can do this using pip:

```bash
pip install firebase-admin
```

## Integrating Firebase with Flask

### Step 1: Initialize Firebase Admin SDK

Create a new directory for your Flask application:

```
flask_firebase_integration/
│
├── app.py
├── requirements.txt
└── serviceAccountKey.json
```

1. **Create a Service Account**:

   - In the Firebase Console, go to **Project settings** > **Service accounts**.
   - Click on **Generate new private key** and download the JSON file. Rename this file to `serviceAccountKey.json` and place it in your project directory.

2. **Create a requirements.txt** file with the following content:

```
Flask
firebase-admin
```

3. **Install the dependencies**:

```bash
pip install -r requirements.txt
```

### Step 2: Set Up the Flask Application

In `app.py`, add the following code to set up the Flask application and initialize the Firebase Admin SDK:

```python
import os
from flask import Flask, jsonify, request
import firebase_admin
from firebase_admin import credentials, firestore

# Initialize Flask app
app = Flask(__name__)

# Initialize Firebase Admin SDK
cred = credentials.Certificate('serviceAccountKey.json')
firebase_admin.initialize_app(cred)

# Initialize Firestore
db = firestore.client()

@app.route('/tasks', methods=['POST'])
def create_task():
    data = request.get_json()
    task_ref = db.collection('tasks').add(data)
    return jsonify({'id': task_ref.id, **data}), 201

@app.route('/tasks', methods=['GET'])
def get_tasks():
    tasks_ref = db.collection('tasks').stream()
    tasks = [{**task.to_dict(), 'id': task.id} for task in tasks_ref]
    return jsonify(tasks)

@app.route('/tasks/<task_id>', methods=['GET'])
def get_task(task_id):
    task_ref = db.collection('tasks').document(task_id).get()
    if task_ref.exists:
        return jsonify({**task_ref.to_dict(), 'id': task_ref.id})
    return jsonify({'message': 'Task not found'}), 404

@app.route('/tasks/<task_id>', methods=['PUT'])
def update_task(task_id):
    task_ref = db.collection('tasks').document(task_id)
    if task_ref.get().exists:
        data = request.get_json()
        task_ref.update(data)
        return jsonify({'id': task_id, **data})
    return jsonify({'message': 'Task not found'}), 404

@app.route('/tasks/<task_id>', methods=['DELETE'])
def delete_task(task_id):
    task_ref = db.collection('tasks').document(task_id)
    if task_ref.get().exists:
        task_ref.delete()
        return jsonify({'message': 'Task deleted'}), 204
    return jsonify({'message': 'Task not found'}), 404

if __name__ == '__main__':
    app.run(debug=True)
```

### Step 3: Run the Application

In your terminal, navigate to the `flask_firebase_integration` directory and run:

```bash
python app.py
```

### Step 4: Testing the API Endpoints

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
     curl -X GET http://127.0.0.1:5000/tasks/<task_id>
     ```

4. **Update a Task**:

   - **Endpoint**: `PUT /tasks/<task_id>`
   - **Description**: Update an existing task by its ID.
   - **Example Request**:
     ```bash
     curl -X PUT http://127.0.0.1:5000/tasks/<task_id> -H "Content-Type: application/json" -d '{"title": "Updated Task", "description": "Updated description"}'
     ```

5. **Delete a Task**:
   - **Endpoint**: `DELETE /tasks/<task_id>`
   - **Description**: Delete a specific task by its ID.
   - **Example Request**:
     ```bash
     curl -X DELETE http://127.0.0.1:5000/tasks/<task_id>
     ```

## Conclusion

In this lesson, we learned how to integrate Firebase with a Flask application. We covered how to set up a Firebase project, initialize the Firebase Admin SDK, and perform CRUD operations using Firestore. This integration allows you to leverage Firebase's powerful features, such as real-time data synchronization and cloud storage, enhancing your application's functionality.

[Next: 09_react_with_typescript](./09_react_with_typescript.md)
