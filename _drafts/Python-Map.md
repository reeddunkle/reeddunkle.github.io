---
layout: post
title: The Map Method
tags:
- Python
- Functional
---

The `map()` method is a way to apply the effect of a function on every element in an iterable. It iterates over the iterable, and passes each element in turn into the function you provide it. Each of these return values are appended to a (new) list, which `map()` returns.

A simple fact about `map()` that I found useful: The iterable it returns will always be the same size of the original.

### Exercise 7: Make a custom map

**Instructions**

- Build a function called `my_map` that takes a function and an interable as arguments.
- The function it takes as an argument should be a function that takes something as an input, does something to it, and returns the changed element
    - It takes a number and returns the number squared
    - It takes a string and returns the string reversed
    - Etc.
- Your `my_map` function should iterate over your iterable, and pass each element into the function you gave it
- It should compile the return values of that step into a new list, which it returns

**Once you've done that**:

- Make a list of numbers: `numbers = range(1, 100)`
- Run this `numbers` through your `my_map` function with an [anonymous function](http://reeddunkle.github.io/Python-Lambda-Closures/) which doubles each number
- Print the output

- Did it work? Here's [**my solution**](https://gist.github.com/reeddunkle/4ff0d639155b4f921d6b0c3a925a63e5)
