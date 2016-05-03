---
layout: post
title: Lambdas (and Closures - Part II)
tags:
- Python
- Beginner to Intermediate
- Closures
---

In Python, anonymous functions are declared with the `lambda` keyword.

### How to use lambdas:

Refer to the following:

```python
add_two_numbers = lambda x, y: x + y

print(add_two_numbers(10, 3))  # => 13
```

- I am setting the variable `add_two_numbers` to be the lambda function
- The variables after the `lambda` declaration are **the function's parameters**: `x, y`
- The resolution of the actions that follow the colon is **its return value**: `x + y`

Written as a traditional function, this would be:

```python
def add_numbers(x, y):
  return x + y
  
add_two_numbers = add_numbers  # To follow the process above
print(add_two_numbers(10, 3))  # => 13
```

----

Because the return value of a lambda isn't explicit, it's useful to see how they are commonly used.

### Lambdas as a boolean expressions

Boolean expressions are one of the most common cases of lambdas.

```python
is_even = lambda x: x % 2 == 0
print(is_even(4))  # => True
print(is_even(3))  # => False
```

```python
is_longer_than_5 = lambda s: len(s) > 5
name1 = "Reed"
name2 = "Reedworth"

print(is_longer_than_5(name1))  # => False
print(is_longer_than_5(name2))  # => True
```

----

### Python Closures - Part II

I will refer to what I covered in my [introduction to closures](http://reeddunkle.github.io/Intro-Closures-Python/). 

The inner functions we make in closures are better expressed with anonymous functions.

While the inner function is doing most of the work in the closure, we never find ourselves needing to refer to it by name.
It ends up taking up a lot space, and it makes things more confusing.

Let's clean up the closures we made in the introduction by replacing the inner functions with lambdas.

Navigate to the `closure_practice` directory (or anywhere you can play with some code).

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

Try this code out, and see if you can do the same with the `exponent` closure we made in Part I.

When you're ready, comment that code out, and try to replace the inner function
of the last example from Part I with a lambda:

```python
def supply_x(x):
    def unnamed_function(func):  # Replace this function with a lambda
        return func(x)

    return unnamed_function
```

----

#### Look for lambdas

Lambda boolean expressions (themselves within closures, at times) are also common as:

- Conditions in list comprehensions
- Conditions in generators
- The condition in these functional methods:
  - [`filter()`](http://reeddunkle.github.io/Python-Filter/), map(), reduce()

I'll write about these in upcoming days, and you'll see better how we put anonymous functions to use.

----

**Further Reading**:

- [Python Closures - Part III](http://reeddunkle.github.io/Python-Closures-Part-3/)
- The _Further Reading_ section from my [first post](http://reeddunkle.github.io/Intro-Closures-Python/) on clousres
