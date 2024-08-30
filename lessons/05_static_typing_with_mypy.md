# Static Typing with MyPy

In this lesson, we will explore static typing in Python using MyPy, a static type checker for Python. Static typing allows you to catch type-related errors during development rather than at runtime, improving code quality and maintainability. We will cover the fundamentals of type annotations in Python, how to use MyPy to check your code, and best practices for leveraging static typing in your projects.

## What is Static Typing?

Static typing is a programming paradigm where variable types are known at compile time. In contrast to dynamic typing, where types are determined at runtime, static typing allows developers to specify the expected types of variables, function parameters, and return values. This can help catch type errors early in the development process, leading to more robust and maintainable code.

### Benefits of Static Typing

1. **Early Error Detection**: By catching type errors during development, static typing helps prevent runtime errors that can lead to application crashes.

2. **Improved Code Readability**: Type annotations serve as documentation, making it easier for developers to understand the expected types of variables and function signatures.

3. **Enhanced IDE Support**: Many modern IDEs provide better code completion and navigation features when type annotations are used, improving developer productivity.

4. **Refactoring Safety**: Static typing makes it easier to refactor code safely, as type checks can help identify unintended consequences of changes.

## Type Annotations in Python

Python introduced type hints in PEP 484, allowing developers to add type annotations to their code. Type annotations can be applied to variables, function parameters, and return values.

### Basic Type Annotations

Here are some basic type annotations in Python:

```python
def add(x: int, y: int) -> int:
    return x + y

result: int = add(5, 3)
```

In this example, we defined a function `add` that takes two integers as parameters and returns an integer. The type annotations specify the expected types.

### Common Built-in Types

Here are some common built-in types you can use in your type annotations:

- `int`: Integer type
- `float`: Floating-point type
- `str`: String type
- `bool`: Boolean type
- `list`: List type
- `dict`: Dictionary type
- `tuple`: Tuple type

### Using `Optional` and `Union`

You can use the `Optional` type from the `typing` module to indicate that a variable may be `None`. Additionally, you can use `Union` to specify that a variable can be one of several types.

```python
from typing import Optional, Union

def get_value(value: Optional[int]) -> Union[str, int]:
    if value is None:
        return "No value provided"
    return value * 2
```

In this example, the `get_value` function accepts an optional integer and returns either a string or an integer.

## Installing MyPy

To get started with MyPy, you need to install it. You can do this using pip:

```bash
pip install mypy
```

## Using MyPy to Check Types

Once you have MyPy installed, you can use it to check your Python files for type errors. Here’s how to do it:

### Step 1: Create a Sample Python File

Create a new directory for your MyPy example:

```
mypy_example/
│
├── example.py
```

In `example.py`, add the following code:

```python
from typing import List

def calculate_average(numbers: List[int]) -> float:
    return sum(numbers) / len(numbers)

# Example usage
avg = calculate_average([1, 2, 3, 4, 5])
print(f"Average: {avg}")

# Intentional type error
avg_error = calculate_average([1, 2, '3', 4, 5])  # This will cause a type error
```

### Step 2: Run MyPy

In your terminal, navigate to the `mypy_example` directory and run MyPy on the `example.py` file:

```bash
mypy example.py
```

You should see output indicating that there is a type error in the `calculate_average` function call with a string in the list:

```
example.py:10: error: Argument 1 to "calculate_average" has incompatible type "List[Union[int, str]]"; expected "List[int]"
```

### Step 3: Fix the Type Error

To fix the type error, ensure that the input to the `calculate_average` function contains only integers:

```python
avg_correct = calculate_average([1, 2, 3, 4, 5])  # This is correct
```

### Step 4: Rerun MyPy

After making the correction, rerun MyPy:

```bash
mypy example.py
```

You should see no errors, indicating that your code is type-safe.

## Best Practices for Using MyPy

Here are some best practices for leveraging MyPy in your projects:

1. **Use Type Annotations Consistently**: Apply type annotations to all your functions, methods, and variables to maximize the benefits of static typing.

2. **Start with Gradual Typing**: If you're working with a large codebase, consider starting with gradual typing. You can add type annotations incrementally and use MyPy to check only parts of the code.

3. **Leverage Type Aliases**: Use type aliases to simplify complex type annotations. This can improve readability and maintainability.

   ```python
   from typing import List, Tuple

   Coordinates = List[Tuple[float, float]]

   def calculate_distance(points: Coordinates) -> float:
       # Implementation here
       pass
   ```

4. **Use MyPy Configuration Files**: You can create a `mypy.ini` configuration file to customize MyPy's behavior, such as ignoring certain errors or specifying paths.

5. **Integrate MyPy into Your Development Workflow**: Consider running MyPy as part of your continuous integration (CI) pipeline to catch type errors before deploying code.

## Example Application: Integrating MyPy with Flask

Let’s create a simple Flask application that demonstrates the use of MyPy for static typing.

### Step 1: Create the Application Structure

Create a new directory for your Flask application:

```
flask_mypy/
│
├── app.py
└── requirements.txt
```

### Step 2: Write the Application Code

In `app.py`, add the following code:

```python
from flask import Flask, jsonify, request
from typing import List

app = Flask(__name__)

@app.route('/sum', methods=['POST'])
def calculate_sum() -> jsonify:
    data = request.json
    numbers: List[int] = data.get('numbers', [])
    total: int = sum(numbers)
    return jsonify({'total': total})

if __name__ == '__main__':
    app.run(debug=True)
```

### Step 3: Run MyPy

In your terminal, navigate to the `flask_mypy` directory and run:

```bash
pip install Flask mypy
mypy app.py
```

You should see no errors, indicating that your code is type-safe.

### Step 4: Test the Endpoint

You can use a tool like Postman or cURL to test the `/sum` endpoint. Send a POST request with the following JSON body:

```json
{
  "numbers": [1, 2, 3, 4, 5]
}
```

If the data is valid, you should receive a response with the total sum:

```json
{
  "total": 15
}
```

## Conclusion

In this lesson, we explored static typing in Python using MyPy. We learned how to use type annotations to specify expected types, how to check for type errors using MyPy, and best practices for leveraging static typing in your projects. We also demonstrated how to integrate MyPy into a Flask application for enhanced type safety.

[Next: 06_restful_apis_with_flask](./06_restful_apis_with_flask.md)
