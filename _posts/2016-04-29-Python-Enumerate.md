---
layout: post
title: Python's Enumerate Method
tags:
- Python
- Beginner to Intermediate
- Iteration
---

This is a follow up to [Python's Iterables and Iterators](http://reeddunkle.github.io/Python-Iterables-Iterators/)

### Enumerate method:

Open up a Python repl:

```bash
>>> the_list = ['a', 'b', 'c']
>>> enum = enumerate(the_list)
>>> enum
<enumerate object at 0x7f4b79751d20>
>>> enum.next()
(0, 'a')
>>> enum.next()
(1, 'b')
>>> enum.next()
(2, 'c')
>>> enum.next()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
>>>
```

#### What it's doing here:

It generates a list of tuples for each element in the list, and generates an iterator of that list.
Each tuple contains first the index number of the element, and second the element.

To demonstrate that it generates a list of tuples, try turning the enumerate object into a list:

```bash
>>> the_list = ['a', 'b', 'c']
>>> enum = enumerate(the_list)
>>> a = list(enum)
>>> a
[(0, 'a'), (1, 'b'), (2, 'c')]
>>> 
```

#### How to take advantage of it:

This is the Pythonic way to utilize elements' index numbers in for-loops:

```bash
>>> the_list = ['a', 'b', 'c']
>>> for index, element in enumerate(the_list):
...     print "Element {} is at index {}.".format(element, index)
... 
Element a is at index 0.
Element b is at index 1.
Element c is at index 2.
>>> 
```

**Note**: This for-loop leverages a feature called "unpacking", which I will explain in a different post.

#### Recap:

- The Python method `enumerate()` generates an iterator of tuples from an iterable.
- The first element in the tuple is the index number of the element in the iterable.
- The second element in the tuple is the actual element in the iterable.



