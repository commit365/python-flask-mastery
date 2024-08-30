# Type Safety with Pydantic

In this lesson, we will explore type safety in Python using Pydantic, a powerful data validation and settings management library. Pydantic allows you to define data models with type annotations, ensuring that the data you work with adheres to specified types and constraints. This lesson will cover the fundamentals of Pydantic, how to create data models, validate data, and integrate Pydantic with Flask applications.

## What is Pydantic?

Pydantic is a library that enforces type hints at runtime and provides data validation and serialization. It is particularly useful for applications that require strict data validation, such as web APIs and data processing pipelines. Pydantic models are defined using Python's type annotations, making it easy to create clear and maintainable data structures.

### Key Features of Pydantic

1. **Data Validation**: Pydantic automatically validates data against the types specified in your models, raising errors if the data does not conform.

2. **Type Inference**: Pydantic uses Python's type hints to infer the types of fields, allowing for clear and concise model definitions.

3. **Serialization and Deserialization**: Pydantic models can be easily serialized to JSON and deserialized from JSON, making it ideal for API development.

4. **Custom Validators**: You can define custom validation logic for fields, allowing for complex validation scenarios.

5. **Integration with FastAPI**: Pydantic is commonly used with FastAPI, but it can also be seamlessly integrated with Flask and other frameworks.

## Installing Pydantic

To get started with Pydantic, you need to install it. You can do this using pip:

```bash
pip install pydantic
```

## Creating Pydantic Models

Pydantic models are created by subclassing `pydantic.BaseModel`. Each attribute of the model is defined with a type annotation. Here’s a simple example:

```python
from pydantic import BaseModel

class User(BaseModel):
    id: int
    name: str
    email: str
```

In this example, we defined a `User` model with three fields: `id`, `name`, and `email`. Each field has a specified type.

### Validating Data

Pydantic automatically validates data when you create an instance of a model. If the data does not conform to the specified types, Pydantic raises a `ValidationError`.

```python
try:
    user = User(id=1, name='Alice', email='alice@example.com')
    print(user)
except ValidationError as e:
    print(e.json())
```

If you try to create a `User` instance with invalid data, such as:

```python
user = User(id='one', name='Alice', email='alice@example.com')
```

Pydantic will raise a `ValidationError` because the `id` field expects an integer.

### Default Values and Optional Fields

You can specify default values for fields and use `Optional` to indicate that a field may be absent.

```python
from typing import Optional

class User(BaseModel):
    id: int
    name: str
    email: str
    age: Optional[int] = None  # Default value is None
```

In this example, the `age` field is optional, and if not provided, it defaults to `None`.

## Custom Validators

Pydantic allows you to define custom validation logic using the `@validator` decorator. This is useful for implementing complex validation rules.

```python
from pydantic import BaseModel, validator

class User(BaseModel):
    id: int
    name: str
    email: str

    @validator('email')
    def email_must_be_valid(cls, value):
        if '@' not in value:
            raise ValueError('Invalid email address')
        return value
```

In this example, we defined a custom validator for the `email` field that checks if the email contains an `@` symbol.

## Serializing and Deserializing Data

Pydantic models can be easily serialized to JSON and deserialized from JSON. This is particularly useful when working with web APIs.

### Serializing to JSON

You can convert a Pydantic model to a JSON string using the `json()` method:

```python
user = User(id=1, name='Alice', email='alice@example.com')
user_json = user.json()
print(user_json)  # Output: {"id": 1, "name": "Alice", "email": "alice@example.com"}
```

### Deserializing from JSON

You can create a Pydantic model instance from a JSON string using the `parse_raw()` method:

```python
json_data = '{"id": 1, "name": "Alice", "email": "alice@example.com"}'
user = User.parse_raw(json_data)
print(user)  # Output: id=1 name='Alice' email='alice@example.com'
```

## Integrating Pydantic with Flask

Pydantic can be seamlessly integrated into Flask applications to validate incoming request data. Here’s how to do it:

### Step 1: Create a Flask Application

Create a new directory for your Flask application:

```
flask_pydantic/
│
├── app.py
└── requirements.txt
```

### Step 2: Write the Application Code

In `app.py`, add the following code:

```python
from flask import Flask, request, jsonify
from pydantic import BaseModel, ValidationError

app = Flask(__name__)

class User(BaseModel):
    id: int
    name: str
    email: str

@app.route('/users', methods=['POST'])
def create_user():
    try:
        user_data = request.json
        user = User(**user_data)  # Validate and create User instance
        return jsonify(user.dict()), 201
    except ValidationError as e:
        return jsonify(e.errors()), 400

if __name__ == '__main__':
    app.run(debug=True)
```

### Step 3: Run the Application

In your terminal, navigate to the `flask_pydantic` directory and run:

```bash
pip install Flask pydantic
python app.py
```

### Step 4: Test the Endpoint

You can use a tool like Postman or cURL to test the `/users` endpoint. Send a POST request with the following JSON body:

```json
{
  "id": 1,
  "name": "Alice",
  "email": "alice@example.com"
}
```

If the data is valid, you should receive a response with the created user:

```json
{
  "id": 1,
  "name": "Alice",
  "email": "alice@example.com"
}
```

If the data is invalid, such as missing the `email` field, you will receive a validation error response:

```json
[
  {
    "loc": ["email"],
    "msg": "field required",
    "type": "value_error.missing"
  }
]
```

## Conclusion

In this lesson, we explored type safety in Python using Pydantic. We learned how to create Pydantic models, validate data, define custom validators, and serialize/deserialize data. We also demonstrated how to integrate Pydantic with a Flask application to enforce data validation for incoming requests.

[Next: 05_static_typing_with_mypy](./05_static_typing_with_mypy.md)
