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

### Example 0: Functions stored in variables

This isn't too complicated, but it's important that you understand that this is a thing that can happen in Python.

```python
def say_hi_to_reed():
    print("Hi Reed")

say_hi_to_reed()  # => "Hi Reed"

variable_holding_a_function = say_hi_to_reed

variable_holding_a_function()  # => "Hi Reed"
```

### Example 1: Intro to Closures

Create a file called example1.py and open it up in the editor of your choice. Here's what I did:

```
touch example1.py
subl example1.py  # Or the editor of your choice
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

With our new closure function `times`, we can remake the function `times_ten`.

Back inside `closure_practice/example1.py`:

```python
# The closure function from above
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
50
```

#### Explaining what is happening

The key for my understanding of this -- when things really clicked for me -- was realizing what `return` is doing at each step. I went through this lesson today with a peer of mine, and this was where he was struggling also. This leads me to believe others will struggle with this too.

Back to the code. Follow the `return` calls:

```python
# The closure function
def times(x):
    def multiply(y):
        return x * y  # When multiply is called, this is what it returns
    
    return multiply  # When times is called, this is what it returns
```

Then we set...
 
```python   
times_ten = times(10)
```

What is this doing? It is setting the variable `times_ten` to the return value of calling `times(10)`.

What is that return value; what does the function `times` return? It returns the function `multiply`.

Then, what does the function `multiply` return? It returns the value of multiplying `x` by `y`.

Go back and track where, and at what point this closure receives the values of `x` and `y`.

When we say...

```python
times_ten = times(10)
```

We are setting `times_ten` to a function (`multiply`) that itself takes a parameter `y`, and which returns the value of `x * y`.

If you go back to the top of your `example1.py` file, and look at the code we commented out, you'll that's exactly what we did to `times_ten`. We simply defined it as a function with the name `times_ten`.

Closures provide us when an additional layer of abstraction. Now, instead of defining a function `times_ten` and `double` and `triple`, we can use our closure function from above, and simply declare:

```python
times_ten = times(10)
double = times(2)
triple = times(3)

print(times_ten(5))  # => 50
print(double(5))  # => 10
print(triple(5))  # => 15
```

----
### Example 2: Try it on your own

```bash
touch example2.py
subl example2.py  # The editor of your choice
```

**Your challenge**:

Inside `closure_practice/example2.py`, try to make a closure function `exponent` that takes a number `y`, and which returns a function that itself takes a number `x`, and which returns the value of raising `x` to the power of `y`.

----
First, you should try to make it work on your own. If you get stuck, I'm going to give hints and work through this as we proceed. If at any point you think that you can figure it out on your own, stopping reading and go try it.

If you're not sure where to begin, start by breaking it down into the chunks like we did above:

1. Make a closure function `exponent` that takes a number `y`
2. Which returns a function that takes a number `x`
3. This function returns the value of raising `x` to the power of `y`

----
If that doesn't help, here is how I would follow these steps:

1.

```python
def exponent(y):
    pass
```

2.

```python
def exponent(y):
    def base(x):
        pass
        
    return base
```

3. (the solution)

```python
def exponent(y):
    def base(x):
        return x**y
        
    return base
```

----
How do you use this closure function to make a function called `square`, that will return the value of squaring a number you give it?

How would you use your closure to make a `cube` function, that cubes a number you give it?

My solutions, using the closure function above:

```python
square = exponent(2)
cube = exponent(3)
```

Looking at what I just typed, what is `square` after I say

```python
square = exponent(2)
```

Do you understand what square is at that moment? Can you track through your code, and point to what you're setting it to?

To make sure that you've got good answers to these questions:

`square` becomes the function `base` from our closure function, except that it has a value for `y` (2). To spell that out with code, when we say `square = exponent(2)`, it is the same as saying:

```python
def square(x):
    return x**2
```












