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

 - Here's [**my solution**](https://gist.github.com/reeddunkle/58e2d9bdb7b41cd3cb796d362964e64c)

### Exercise 6

Take this list of tuples:

```python
members = [('name1', 'Reed'), ('song1', "Don't Turn Around"), ('name2', 'Jonathan'), ('song2', "All That She Wants")]
```

Use your `my_filter` method to build a new list containing only the tuples that represent the members' names.

Hints:

- Elements in tuples can be accessed with index references
    - [Example](https://gist.github.com/reeddunkle/dc0333e1e32af46c2b7fd5745ded924e)
- The `startswith()` method is super useful here. It takes a string as an argument, and compares that string against the start of the string on which it was called. If the characters all match, it returns `True`, otherwise it returns `False`.
    - [Example](https://gist.github.com/reeddunkle/6040173f8d1b5202998afedd2642e3f4)

- [**My solution**](https://gist.github.com/reeddunkle/569b6eb11340292be398faa5c431ac6d)

### The built-in `filter()` method

The built-in method:

_filter(function, iterable)_

Now that you've got a working version of your custom filter method, let's look briefly at the advantages of the built-in method.

**Advantage 1**

From the [documentation](https://docs.python.org/2/library/functions.html#filter)

> If iterable is a string or a tuple, the result also has that type; otherwise it is always a list.

Our function always filters the iterable's elements into a list. If you filter a tuple or a string, though, the output should be the same.

Example

```python
def my_filter(function, iterable):
    output = []
    for item in iterable:
        if function(item):
            output.append(item)

    return output

the_tuple = (1, 2, 3, 4, 5, 6, 7, 8, 9)

result1 = my_filter(lambda x: x % 2 == 0, the_tuple)
result2 = filter(lambda x: x % 2 == 0, the_tuple)

print(result1)
print(result2)
```

The output:

```bash
[2, 4, 6, 8]  # my_filter()
(2, 4, 6, 8)  # filter()
```


**Advantage 2**

From the [documentation](https://docs.python.org/2/library/functions.html#filter):

> filter(function, iterable) is equivalent to [item for item in iterable if function(item)] if function is not None and [item for item in iterable if item] if function is None.

With the built-in filter, if you pass it `None` for the function in its parameters, it defaults to filtering the iterable's elements based on their truthiness, `if element`.

Adjusting `my_filter` to support this feature:

```python
def my_filter(function, iterable):
    output = []
    for item in iterable:
        if function:
            if function(item):
                output.append(item)
        elif item:
            output.append(item)

    return output


the_list = [True, False, 100, "Reed", None, None, "Reedworth"]

result1 = my_filter(None, the_list)
result2 = filter(None, the_list)  # Showing the built-in

print(result1)
print(result2)
```

The output:

```bash
[True, 100, 'Reed', 'Reedworth']
[True, 100, 'Reed', 'Reedworth']
```

