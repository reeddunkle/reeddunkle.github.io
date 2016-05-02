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

I am setting the variable `add_two_numbers` to be the lambda function.

The variables that follow the `lambda` declaration are the functions parameters: `x, y`
The resolution of the actions that follow the colon is its return value: `x + y`

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

print(is_longer_than_5(name1))
print(is_longer_than_5(name2))
```

----

### Closures: Part II

I will refer to what I covered in Part I, the [Introduction to Closures](http://reeddunkle.github.io/Intro-Closures-Python/). 

The inner functions we make in closures are better expressed with anonymous functions.

While the inner function is doing most of the work in the closure, we never refer to the inner functions by name.
It ends up taking up a lot space, and making things more confusing.

Let's clean up the closures we made in the introduction by replacing the inner functions with lambdas:

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

#### Last words

As I said, the most common examples of lambdas that I've found are to create easy boolean expressions and to create closures. Lambda boolean expressions in particular (themselves within closures, at times) are common in these other subjects:

- As conditions in list comprehensions
- As conditions in generators
- As the condition in these functional methods:
  - filter(), map(), reduce()

I'll write about these in upcoming days, and you'll see better how we put anonymous functions to use.