---
layout: post
title: Introduction to Closures in Python
tags:
- Python
- Closures
- Beginner to Intermediate
---

**Proficiency**: Beginner to Intermediate; I'm going to assume that closures are confusing to you.

**Specifications**: I'm using Python 2.7.6

In your shell, navigate to a directory where you can make some sample code files.
If you don't have one that you already use for this purpose, make a directory called `closure_practice` and navigate into that directory:

```bash
cd ~
mkdir closure_practice
cd closure_practice
```

I've added this next section in my latest draft of this post, because I realized that it's important that you understand two things before we start on closures.

### Exercise 0.1: Functions stored in variables

This is a thing that can happen in Python:

```python
def say_hi_to_reed():
    print("Hi Reed")

say_hi_to_reed()  # => "Hi Reed"

variable_holding_a_function = say_hi_to_reed

variable_holding_a_function()  # => "Hi Reed"
```

### Exercise 0.2: Functions defined inside another function

This is another thing that can happen in Python:

```python
def say_hi():

    def provide_name():
        return "Reed"

    name = provide_name()
    print "Hi {}".format(name)

say_hi()
```

Make sure you understand how `say_hi` constructs its output string from the function `provide_name` that is defined within it.

----

### Exercise 1: Intro to Closures

Create a file (inside of the directory we mentioned above) called exercise1.py and open it up in the editor of your choice (I use Sublime Text). Here's what I did:

```
touch exercise1.py
subl exercise1.py  # Note: subl is Sublime Text's shorthand to open things up into it
```

Let's say that we want to make a function that takes a number `x` and returns the value of multiplying that number by 10.

In `closure_practice/exercise1.py`:

```python
def times_ten(x):
    return x * 10

result = times_ten(5)
print(result)
```

In your shell, run it:

```bash
python exercise1.py
50
```

So far so good. Now let's make a function that takes two numbers `x` and `y`, and returns their product.

Back inside `closure_practice/exercise1.py`:

```python
def multiply(x, y):
    return x * y

result = multiply(5, 10)
print(result)
```

In your shell:

```bash
python exercise1.py
50
```

Right, so comment all of that out, and it's on to the next one. (I recommend commenting out, rather than deleting these early portions, so that you have them to refer to in the same file.)

### Closures:

What if we want to make a function `times` that takes a number `x`, and which returns a function that itself takes a number `y`, and this function returns the value of multiplying `x` and `y`?

Let's step through this by breaking that down:

- Make a function `times` that takes a number `x`:

```python
def times(x):
    pass
```

- Which returns a function that itself takes a number `y`...

```python
def times(x):
    def multiply(y):
        pass

    return multiply
```

- And this function returns the value of multiplying `x` and `y`:

```python
def times(x):
    def multiply(y):
        return x * y

    return multiply
```

----

This is an example of a closure. A closure describes the relationship the inner function `multiply` has to its parent function `times`. It includes the dependence `multiply` has on `times` to fulfill the lexical scope requirements of its calculations -- (`times` provides `multiply` with `x`).

I'm going to go into more depth in this in a later post.

Remember our `times_ten` function above? It took a number `x` and returned the value of multiplying `x` and `10`.

With our new closure function `times`, we can remake the function `times_ten`.

**Note**: The advantage doing this is reffered to as _function portability_, or making _higher order functions_, or more generally _abstraction_.

Back inside `closure_practice/exercise1.py`:

```python
# The closure function from above
def times(x):
    def multiply(y):
        return x * y

    return multiply


times_ten = times(10)  # Can you explain what this sets times_ten to?

result = times_ten(5)  # This is the code from the top of exercise1.py
print(result)
```

Go to your shell and run it:

```bash
python exercise1.py
50
```


### Explaining what is happening


The key for my understanding this -- when things really clicked for me -- was realizing what `return` is doing at each step.

Before writing this, I went through these exercises with a peer of mine, and this was where he was struggling also. This leads me to believe others will struggle with this too.

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

If you go back to the top of your `exercise1.py` file, and look at the code we commented out, you'll that's exactly what we did to `times_ten`. We simply defined it as a function with the name `times_ten`.

Closures provide us with an additional layer of abstraction. Now, instead of defining a function `times_ten`, `double` and `triple`, we can use our closure function from above, and simply declare:

```python
times_ten = times(10)
double = times(2)
triple = times(3)

print(times_ten(5))  # => 50
print(double(5))  # => 10
print(triple(5))  # => 15
```

----

### Exercise 2: Try it on your own

```bash
touch exercise2.py
subl exercise2.py  # The editor of your choice
```

**Your challenge**:

Inside `closure_practice/exercise2.py`, try to make a closure function `exponent` that takes a number `y`, and which returns a function that itself takes a number `x`, and which returns the value of raising `x` to the power of `y`.

----
First, you should try to make it work on your own. If you get stuck, I'm going to give hints and work through this as we proceed. If at any point you think that you can figure it out on your own, stopping reading and go try it.

If you're not sure where to begin, start by breaking it down into the chunks like we did above:

1. Make a closure function `exponent` that takes a number `y`
2. Which returns a function that takes a number `x`
3. This function returns the value of raising `x` to the power of `y`

----
If that doesn't help, here is how I would follow these steps:

**One**:

```python
def exponent(y):
    pass
```

**Two**:

```python
def exponent(y):
    def base(x):
        pass

    return base
```

**Three (the solution)**:

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

Looking at what I just typed, what is `square` after I declare...?

```python
square = exponent(2)
```

Do you understand what `square` is at that moment? Can you track through your code, and point to what you're setting it to?

To make sure that you've got good answers to these questions:

`square` becomes the function `base` from our closure function, except that it has a value for `y` (2). To spell that out with code, when we say `square = exponent(2)`, it is the same as saying:

```python
def square(x):
    return x**2
```

----

### Exercise 3: Interpreting someone else's closure

Try to work through the following closure on your own. Use the tactics we used above.

```python
def supply_x(x):
    def unnamed_function(func):
        return func(x)

    return unnamed_function

supply_five = supply_x(5)
```

- Can you write a simple example that puts `supply_five` to use?
- Can you make an example that uses `supply_x` with something other than a number, and then puts that to use?
- With the first two examples, I started with instructions, "Write a function that take a number, and which returns a function..." Can you write what the instructions for the above closure would be?

If you can do these things, I think you've got the hang of closures.

I haven't written it yet, but my next post will be on anonymous functions, and I'll revisit this example when I write that.

If you're confused, don't worry. It's confusing.

To start working out the confusing parts, try to answer these questions:

1. After that last line, to what is `supply_five` set? Work backwards from there.
2. What is each function returning?
3. What are the parameters of each function; what does each function require be passed in?
4. Where and when are the parameters of the functions being used?

**Spoiler ahead**:

If you're still confused, I want to walk you through it. Here's the code again for reference:

```python
def supply_x(x):
    def unnamed_function(func):
        return func(x)

    return unnamed_function

supply_five = supply_x(5)
```

Answering the above questions:

- `supply_five` is set to the return value of calling `supply_x(5)`
- `supply_x` returns `unnamed_function`
- Okay, then what is `unnamed_function`? It is a function that takes `func` as a parameter, and returns `func(x)`

That's strange, because we don't explicitly know what `func` is.

We can deduce what it is, though, right? It's called `func`, which isn't meant to trick you. It's probably a function. And the return value `func(x)` reinforces that. We call functions with parenthesis, passing in their parameters as required.

You may feel that the code above assumes too much. You're right, it does. As an exercise, though, once you understand what each part does and needs, it isn't too hard to make an example that puts `supply_x` to use.

Before I say more, see if you can put `supply_x` to use. Feel free to use `supply_five`.

Returning to the explanation:

- `supply_x` returns `unnamed_function`
- `unnamed_function` takes the parameter `func`, and returns the value of calling `func(x)`.

This feels strange, because we don't know what `func` might be. Okay, but what do we know? We know that we can pass `unnamed_function` a function, and it will return the value of calling the function we passed into it with the value of `x`.

If we say `supply_five = supply_x(5)`, then `supply_five` will call whatever function it is given with 5 as the parameter.

**How do we put this to use?**

Let's make a function that takes in a number, and returns the value of doing something with that number.

Go to `closure_practice/exercise3.py` and add the above code if you haven't already:

```python
def supply_x(x):
    def unnamed_function(func):
        return func(x)

    return unnamed_function

supply_five = supply_x(5)

# We make something that can use supply_five

def multiply_by_ten(x):
    return x * 10

# Now we use supply_five

test_result = supply_five(multiply_by_ten)

print test_result
```

Run the code:

```bash
python exercise3.py
50
```

Now go back to the closure, and write an example like the one above that uses a string instead of a number.

If you're still confused, let me know and I'll work with you to make this post better.

### Final comments

I'm left feeling that I've spent all of the above time focusing on useless examples, not teaching anything useful. This is partly (or mostly) because I am new to all of this myself, and I don't know enough to appreciate the power of these tools.

That said, I do know that, conceptually, the reason closures are powerful is because of the additional layer of abstraction they grant us. I've heard this called "higher-order functions".

I hope that makes sense, conceptually. We can make a function that takes a number and multiplies it by ten. Or we can make a function that lets us create other functions to accomplish various aspects of a given task.

I will revisit and update this as I learn more. In the meantime, I've learned more by looking at:

----

**Further reading**:

- [Closures Part II: Anonymous Functions](http://reeddunkle.github.io/Python-Lambda-Closures/)
- Mozilla's [post about closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures) (in JavaScript). They provide some practical examples.
- [The 10 Most Common Mistakes That Python Developers Make](https://www.toptal.com/python/top-10-mistakes-that-python-programmers-make), check out _Common Mistake #6: Confusing how Python binds variables in closures_
- My friend's [Closure practice in Ruby](https://github.com/natalieparellano/programming-practice/blob/master/closures.rb)
- Read the chapter "First Class Functions" in [this article](http://wit.io/posts/ruby-is-beautiful-but-im-moving-to-python)
