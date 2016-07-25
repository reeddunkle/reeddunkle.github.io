---
layout: post
title: Python Generators
tags:
- Python
---

When I first heard about generators, it was in context of Python2, and the difference between **range()** and **xrange()**, `xrange()` being a generator function. In Python3, I learned, the original `range()` was removed, and `xrange()` was renamed `range()`. If you look up generators in Python, I can almost guarantee this will be the example you'll be given.

I'm going to briefly cover this, and then move onto more interesting things.

If you haven't used Python3 much, then you're accustomed to this:

```
>>> a = range(5)
>>> a
[0, 1, 2, 3, 4]
>>>
>>> a = xrange(5)
>>> a
xrange(5)
```

With the original `range()` function, it returns a list. This means that the entire list is help in memory. On the other hand, `xrange()`, being a generator, only returns one element at a time
