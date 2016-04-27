---
layout: post
title: Closures in Python
---

**Specifications**: I'm using Python 2.7.6

I'm going to assume that you're as new to closures as I was a week ago.

In your shell, navigate to a directory where you can make some sample code files.
If you don't have one that you already use for this purpose, make a directory called `closure_practice` and navigate into that directory:

```bash
cd ~
mkdir closure_practice
cd closure_practice
```

### Example 1:

Create a file called example1.py and open it up in the editor of your choice. Here's what I did:

```
touch example1.py
subl example1.py
```

Let's say that we want to make a function that takes a number `x` and returns the value of multiplying that number by 10.

In `closure_practice/example1.py`:

```python
def times_ten(x):
    return x * 10

result = times_ten(5)
print(result)
```

In your shell:

```bash
python example1.py
50
```

So far so good. Now let's make a function that takes two numbers `x` and `y`, and returns their product.

(I recommend commenting out these early portions so that you have them to refer to in the same file.)

Back inside `closure_practice/example1.py`:

```python
def multiply(x, y):
    return x * y
    
result = multiply(5, 10)
print(result)
```

In your shell:

```bash
python example1.py
50
```

Right, so comment all of that out, and it's on to the next one:

#### Closures:

What if we want to make a function `times` that takes a number `x`, and which returns a function that itself takes a number `y`, and this function returns the value of multiplying `x` and `y`?

Let's step through this by breaking that down:

1. Make a function `times` that takes a number `x`:

```python
def times(x):
    pass
```

2. Which returns a function that itself takes a number `y`...

```python
def times(x):
    def multiply(y):
        pass
        
    return multiply
```

3. And this function returns the value of multiplying `x` and `y`:

```python
def times(x):
    def multiply(y):
        return x * y
    
    return multiply
```

This is a closure. A function that encloses another function. The enclosed function is dependent on the enclosing function.
Remember our `times_ten` function above? It took a number `x` and returned the value of multiplying `x` and `10`.

With our new closure function `times`, we can remake the `times_ten` function.

Back inside `closure_practice/example1.py`:

```python
# The closure from above
def times(x):
    def multiply(y):
        return x * y
    
    return multiply

    
times_ten = times(10)  # Can you explain what this sets times_ten to?

result = times_ten(5)  # This is the code from the top of example1.py
print(result)
```

Go to your shell and run it:

```bash
python example1.py
