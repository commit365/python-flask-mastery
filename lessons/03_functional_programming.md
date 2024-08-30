# Functional Programming in Python

In this lesson, we will explore the principles and practices of functional programming (FP) in Python. Functional programming is a programming paradigm that treats computation as the evaluation of mathematical functions and avoids changing state or mutable data. This lesson will cover the core concepts of functional programming, including first-class functions, pure functions, higher-order functions, and the use of built-in functional programming tools in Python.

## What is Functional Programming?

Functional programming is a declarative programming paradigm where the focus is on "what" to solve rather than "how" to solve it. This paradigm emphasizes the use of functions as the primary building blocks of programs. In functional programming, functions are treated as first-class citizens, meaning they can be passed as arguments, returned from other functions, and assigned to variables.

### Key Concepts of Functional Programming

1. **Pure Functions**: A pure function is a function that always produces the same output for the same input and has no side effects. This means that pure functions do not modify any external state or variables. For example:

   ```python
   def add(x, y):
       return x + y
   ```

   The `add` function is a pure function because it does not alter any external state and always returns the same result for the same inputs.

2. **First-Class Functions**: In Python, functions are first-class citizens. This means you can treat functions like any other object. You can assign them to variables, pass them as arguments, and return them from other functions.

   ```python
   def greet(name):
       return f"Hello, {name}!"

   greeting = greet  # Assigning function to a variable
   print(greeting("Alice"))  # Output: Hello, Alice!
   ```

3. **Higher-Order Functions**: A higher-order function is a function that takes one or more functions as arguments or returns a function as its result. This allows for powerful abstractions and code reuse.

   ```python
   def apply_function(func, value):
       return func(value)

   result = apply_function(add, 5)  # Assuming `add` takes a single argument
   ```

4. **Immutability**: In functional programming, data is often treated as immutable. Once a variable is assigned a value, it cannot be changed. Instead of modifying data, new data structures are created.

   ```python
   numbers = [1, 2, 3]
   new_numbers = numbers + [4]  # Creates a new list
   ```

5. **Recursion**: Functional programming often uses recursion as a primary means of iteration instead of traditional loops. A recursive function calls itself to solve smaller instances of the same problem.

   ```python
   def factorial(n):
       if n == 0:
           return 1
       return n * factorial(n - 1)
   ```

## Functional Programming Tools in Python

Python provides several built-in functions and modules that facilitate functional programming. Here are some of the most commonly used tools:

### 1. Lambda Functions

Lambda functions are small anonymous functions defined using the `lambda` keyword. They can take any number of arguments but can only have one expression.

```python
square = lambda x: x ** 2
print(square(5))  # Output: 25
```

### 2. `map()`

The `map()` function applies a given function to all items in an iterable (e.g., list) and returns an iterator.

```python
numbers = [1, 2, 3, 4]
squared = list(map(lambda x: x ** 2, numbers))
print(squared)  # Output: [1, 4, 9, 16]
```

### 3. `filter()`

The `filter()` function constructs an iterator from elements of an iterable for which a function returns true.

```python
numbers = [1, 2, 3, 4, 5]
even_numbers = list(filter(lambda x: x % 2 == 0, numbers))
print(even_numbers)  # Output: [2, 4]
```

### 4. `reduce()`

The `reduce()` function from the `functools` module applies a rolling computation to sequential pairs of values in an iterable. It reduces the iterable to a single cumulative value.

```python
from functools import reduce

numbers = [1, 2, 3, 4]
product = reduce(lambda x, y: x * y, numbers)
print(product)  # Output: 24
```

## Combining Functional Programming with Flask

As you develop applications using Flask, you can incorporate functional programming principles to enhance code readability, maintainability, and testability. Here are some ways to apply functional programming concepts in your Flask applications:

1. **Use Pure Functions**: Ensure that your route handlers and utility functions are pure. This makes them easier to test and reason about.

2. **Higher-Order Functions for Middleware**: You can create higher-order functions to implement middleware that modifies request and response objects.

3. **Functional Composition**: Compose functions together to build complex functionality from simpler functions.

4. **Immutability**: Use immutable data structures where possible to avoid unintended side effects.

## Example Application: Functional Programming in Action

Let’s create a simple Flask application that demonstrates the use of functional programming concepts.

### Step 1: Create the Application Structure

Create a new directory for your Flask application:

```
flask_functional/
│
├── app.py
└── requirements.txt
```

### Step 2: Write the Application Code

In `app.py`, add the following code:

```python
from flask import Flask, jsonify
from functools import reduce

app = Flask(__name__)

# Pure function to calculate the square of a number
def square(x):
    return x ** 2

# Pure function to filter even numbers
def is_even(x):
    return x % 2 == 0

# Higher-order function to apply a function to a list
def apply_function_to_list(func, lst):
    return list(map(func, lst))

@app.route('/squares/<int:n>')
def get_squares(n):
    numbers = list(range(n))
    squares = apply_function_to_list(square, numbers)
    return jsonify(squares)

@app.route('/evens/<int:n>')
def get_evens(n):
    numbers = list(range(n))
    evens = list(filter(is_even, numbers))
    return jsonify(evens)

@app.route('/product/<int:n>')
def get_product(n):
    numbers = list(range(1, n + 1))
    product = reduce(lambda x, y: x * y, numbers)
    return jsonify(product)

if __name__ == '__main__':
    app.run(debug=True)
```

### Step 3: Run the Application

In your terminal, navigate to the `flask_functional` directory and run:

```bash
pip install Flask
python app.py
```

### Step 4: Test the Endpoints

Open your web browser or use a tool like Postman to test the following endpoints:

- `http://127.0.0.1:5000/squares/5` will return the squares of numbers from 0 to 4.
- `http://127.0.0.1:5000/evens/10` will return the even numbers from 0 to 9.
- `http://127.0.0.1:5000/product/5` will return the product of numbers from 1 to 5.

## Conclusion

In this lesson, we explored the principles of functional programming in Python, including pure functions, first-class functions, higher-order functions, and the use of built-in functional programming tools. We also demonstrated how to apply these concepts in a Flask application to create clean, maintainable code.

[Next: 04_type_safety_with_pydantic](./04_type_safety_with_pydantic.md)
