---
layout: post
title: The Filter Method
tags:
- Python
- Functional
---

The `filter()` method is a way to filter through an [iterable](http://reeddunkle.github.io/Python-Iterables-Iterators/), using a boolean function as a sieve. It constructs a new iterable, filled with the elements from the original iterable that pass the criterion of your boolean function.

It's a quick, easy way to test every element in your iterable against a certain criterion -- those that pass are passed on to the output, those that fail are left out.

### Exercise 5: Make a custom filter

I've found that making my own versions of built-in methods helps me understand the built-in methods a lot better. You should try to do it:

**Instructions**

 - Build a function called `my_filter` that takes a boolean function and a list as parameters. The function should iterate over the list, and test each element in the list against the boolean. Elements for which the boolean expression is `True` should be added to a new list. The function should return the new list.
 
 Once you've done that:
 
 - Set make a list of numbers: `numbers = range(1, 100)`
 - Print the result of calling `my_filter` with a [lambda function](http://reeddunkle.github.io/Python-Lambda-Closures/) that filters the elements in `numbers` which are divisible by two.
 
 Here's [my solution](https://gist.github.com/reeddunkle/58e2d9bdb7b41cd3cb796d362964e64c)

### Exercise 6

Take this list of tuples:

```python
members = [('name1', 'Reed'), ('song1', "Don't Turn Around"), ('name2', 'Jonathan'), ('song2', "All That She Wants")]
```

Use your `my_filter` method to build a new list containing only the tuples that represent the members' names.

Hints:

- Elements in tuples can be accessed with index references

```bash
>>> tuple = ('fashion', 'nugget')
>>> tuple[0]
'fashion'
>>> tuple[1]
'nugget'
```


- The `startswith()` method is super useful here. It takes a string as an argument, and compares that string against the start of the string on which it was called. If the characters all match, it returns `True`, otherwise it returns `False`.

Here's an example:

<script src="https://gist.github.com/reeddunkle/6040173f8d1b5202998afedd2642e3f4.js"></script>


### The actual, built-in filter

Now that you've got a working version of your custom filter method, let's look briefly at the advantages of using the built-in `filter()` method.

- Our function only
