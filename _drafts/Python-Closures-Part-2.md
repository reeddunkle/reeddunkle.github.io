---
layout: post
title: Closures Part II
tags:
- Python
- Closures
- Intermediate
---

I will refer to what I covered in the [Introduction to Closures](http://reeddunkle.github.io/Intro-Closures-Python/).

Navigate to the `closure_practice` directory (or anywhere you can play with some code).

### Exercise 4: Lambdas in Closures

The inner functions we make in closures are better expressed with anonymous functions. While the inner function
is doing most of the work in the closure, we don't need to refer to the inner functions by name.

It ends up taking up a lot space, and making things more confusing.

Let's clean up the closures we made in the introduction:

```bash
subl exercise4.py  # Or the editor of your choice
```

Remember this code?

```python
def times(x):
    def multiply(y):
        return x * y
        
    return multiply
```

To convert the inner function to a lambda function:

```python
def times(x):
  return lambda y: x * y
```

Comment that code out, and try to replace the inner functions of the last example from the introduction with a lambda:

```python
def supply_x(x):
    def unnamed_function(func):  # Replace this function with a lambda
        return func(x)

    return unnamed_function
```


