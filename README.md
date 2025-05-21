# Python Decorators

A decorator in Python is a design pattern that allows you to modify or extend the behavior of a function, method, or class **without permanently altering its code**. Decorators are implemented as functions (or classes) that "wrap" other functions/classes, enabling reusable and flexible code.

---

## Table of Contents
- [What is a Decorator?](#what-is-a-decorator)
- [Basic Syntax](#basic-syntax)
- [Example: Logging Decorator](#example-logging-decorator)
- [Common Use Cases](#common-use-cases)
- [Chaining Decorators](#chaining-decorators)
- [Decorators with Parameters](#decorators-with-parameters)
- [Preserving Metadata with `functools.wraps`](#preserving-metadata-with-functoolswraps)
- [Learn More](#learn-more)

---

## What is a Decorator?

A decorator is a **higher-order function** that:

1. Takes a function as an argument.
2. Returns a new function with enhanced or modified behavior.
3. Uses the `@decorator_name` syntax to apply it to a target function or class.

---

## Basic Syntax

```python
def my_decorator(func):
    def wrapper():
        print("Before function execution.")
        func()
        print("After function execution.")
    return wrapper

@my_decorator
def greet():
    print("Hello, World!")

greet()
```

**Output:**
```
Before function execution.
Hello, World!
After function execution.
```

---

## Example: Logging Decorator

```python
def logger(func):
    def wrapper(*args, **kwargs):
        print(f"Executing: {func.__name__}")
        result = func(*args, **kwargs)
        print(f"Finished executing: {func.__name__}")
        return result
    return wrapper

@logger
def add(a, b):
    return a + b

print(add(2, 3))
```

**Output:**
```
Executing: add
Finished executing: add
5
```

---

## Common Use Cases

- **Logging:** Track function calls and outputs.
- **Timing:** Measure execution time.
- **Authentication:** Restrict access to certain functions.
- **Caching/Memoization:** Store results of expensive calls.
- **Validation:** Check input/output formats.

---

## Chaining Decorators

You can apply multiple decorators to a single function. The order matters as decorators are applied from the innermost outward.

**Example:**

```python
def bold(func):
    def wrapper():
        return "<b>" + func() + "</b>"
    return wrapper

def italic(func):
    def wrapper():
        return "<i>" + func() + "</i>"
    return wrapper

@bold
@italic
def greet():
    return "Hello"

print(greet())
```

**Output:**
```
<b><i>Hello</i></b>
```

---

## Decorators with Parameters

To pass arguments to a decorator, nest another function:

```python
def repeat(n):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for _ in range(n):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator

@repeat(3)
def say_hello():
    print("Hello!")

say_hello()
```

**Output:**
```
Hello!
Hello!
Hello!
```

---

## Preserving Metadata with `functools.wraps`

Decorators can obscure the original function's metadata (e.g., `__name__`, `__doc__`). Use `functools.wraps` to retain this information:

```python
from functools import wraps

def timer(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        import time
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"{func.__name__} took {end - start:.2f}s")
        return result
    return wrapper

@timer
def calculate():
    """Example function for calculation."""
    return 42 * 42

print(calculate.__name__)
print(calculate.__doc__)
```

**Output:**
```
calculate
took 0.00s
Example function for calculation.
```

---

## Learn More

- [Python Decorators Documentation](https://docs.python.org/3/glossary.html#term-decorator)
- [Real Python Guide on Decorators](https://realpython.com/primer-on-python-decorators/)
