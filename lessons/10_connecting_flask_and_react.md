# Connecting Flask and React

In this lesson, we will explore how to connect a Flask backend with a React frontend to create a full-stack web application. We will cover the process of setting up both the Flask API and the React application, enabling Cross-Origin Resource Sharing (CORS), making API requests from React, and displaying the data in the user interface.

## Overview

The integration of Flask and React allows developers to take advantage of Flask's powerful backend capabilities while leveraging React's dynamic user interface features. This lesson will guide you through the following steps:

1. Setting up a Flask API.
2. Enabling CORS for cross-domain requests.
3. Creating a React application.
4. Making API requests from React to Flask.
5. Displaying the fetched data in the React application.

## Step 1: Setting Up a Flask API

### 1.1 Create a Flask Project

First, create a new directory for your Flask backend:

```bash
mkdir flask_backend
cd flask_backend
```

### 1.2 Create a Virtual Environment

Create a virtual environment to manage your dependencies:

```bash
python -m venv venv
```

Activate the virtual environment:

- On macOS/Linux:

  ```bash
  source venv/bin/activate
  ```

- On Windows:

  ```bash
  venv\Scripts\activate
  ```

### 1.3 Install Flask and Flask-CORS

Install Flask and Flask-CORS to handle CORS:

```bash
pip install Flask Flask-CORS
```

### 1.4 Create the Flask Application

Create a new file called `app.py` and add the following code:

```python
from flask import Flask, jsonify
from flask_cors import CORS

app = Flask(__name__)
CORS(app)  # Enable CORS for all routes

@app.route('/api/data', methods=['GET'])
def get_data():
    return jsonify({'message': 'Hello from Flask!'})

if __name__ == '__main__':
    app.run(debug=True)
```

### 1.5 Run the Flask Application

Run the Flask application:

```bash
python app.py
```

Your Flask API should now be running at `http://127.0.0.1:5000/api/data`.

## Step 2: Creating a React Application

### 2.1 Set Up the React Project

Open a new terminal window and create a new React application using Create React App:

```bash
npx create-react-app react_frontend
cd react_frontend
```

### 2.2 Configure Proxy in React

To avoid CORS issues during development, you can set up a proxy in your React application. Open the `package.json` file and add the following line:

```json
"proxy": "http://localhost:5000",
```

This tells the React development server to proxy requests to the Flask server running on port 5000.

### 2.3 Install Axios

Install Axios, a promise-based HTTP client for making API requests:

```bash
npm install axios
```

## Step 3: Making API Requests from React

### 3.1 Create a Component to Fetch Data

Create a new file called `DataFetcher.js` in the `src` directory:

```jsx
import React, { useEffect, useState } from "react";
import axios from "axios";

const DataFetcher = () => {
  const [data, setData] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await axios.get("/api/data");
        setData(response.data.message);
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    };

    fetchData();
  }, []);

  return (
    <div>
      <h1>{data ? data : "Loading..."}</h1>
    </div>
  );
};

export default DataFetcher;
```

### 3.2 Use the DataFetcher Component in Your App

Open the `src/App.js` file and update it to include the `DataFetcher` component:

```jsx
import React from "react";
import DataFetcher from "./DataFetcher";

const App = () => {
  return (
    <div>
      <h1>Flask and React Integration</h1>
      <DataFetcher />
    </div>
  );
};

export default App;
```

## Step 4: Run the React Application

In the terminal, navigate to the `react_frontend` directory and start the React application:

```bash
npm start
```

Your React application should now be running at `http://localhost:3000`, and it should display the message fetched from the Flask backend.

## Step 5: Testing the Integration

1. Ensure that both the Flask server and the React application are running.
2. Open your browser and navigate to `http://localhost:3000`.
3. You should see "Flask and React Integration" followed by the message "Hello from Flask!" displayed on the page.

## Conclusion

In this lesson, we successfully connected a Flask backend with a React frontend. We set up a Flask API to serve data, enabled CORS to allow cross-domain requests, created a React application to fetch and display that data, and demonstrated the seamless interaction between the two technologies.

This integration opens up many possibilities for building robust full-stack applications.

[Next: 11_deployment_strategies](./11_deployment_strategies.md)
