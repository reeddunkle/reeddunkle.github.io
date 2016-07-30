---
layout: post
title: Python Generators Part II - Under the hood
tags:
- Python
- Generators
---

The main concept that has helped me grasp and use generators is the idea that a generator is an iterable function. I'm going to poke around briefly in the repl to show what methods are available to generators, and how Python uses them.

What is the original `range` at its core? It's a function that returns a list of numbers. What is `xrange` at its core? It's a function that...what? Brings one element into memory at a time. Okay, but what does that mean?

Let's look. I'm going to switch to [iPython](https://ipython.org/install.html) because it's much easier on the eyes. If you've never used iPython before, don't worry. It's just a repl with a different look:

```
In [1]: a = xrange(5)

In [2]: type(a)
Out[2]: xrange

In [3]: b = iter(a)

In [4]: type(b)
Out[4]: rangeiterator

In [5]: dir(b)
Out[5]:
['__class__',
 '__delattr__',
 '__doc__',
 '__format__',
 '__getattribute__',
 '__hash__',
 '__init__',
 '__iter__',
 '__length_hint__',
 '__new__',
 '__reduce__',
 '__reduce_ex__',
 '__repr__',
 '__setattr__',
 '__sizeof__',
 '__str__',
 '__subclasshook__',
 'next']

In [6]: next(b)
Out[6]: 0

In [7]: next(b)
Out[7]: 1
```

It looks like `xrange` returns an 'xrange' object **which is an iterable**. An [iterable](http://reeddunkle.github.io/Python-Iterables-Iterators/) is an object that can return an iterator. An iterator has the method `next()`.

On the other hand, (the original) `range` returns a list, which is an iterable. Let's imagine you want to use range in a for-loop like above:

```
In [8]: squares = []

In [9]: for x in range(5):
   ...:     squares.append(x)
   ...:

In [10]: squares
Out[10]: [0, 1, 2, 3, 4]
```

You'll remember this from the [article](http://reeddunkle.github.io/Python-Iterables-Iterators/) on iterables and iterators. This is roughly what's happening when you iterate over a list, like in the above code:

```
In [11]: rng = range(5)

In [12]: iterator_rng = iter(rng)

In [13]: squares = []

In [14]: while 1:
    ...:     try:
    ...:         sqr = next(iterator_rng) ** 2
    ...:         squares.append(sqr)
    ...:     except StopIteration:
    ...:         break
    ...:

In [15]: squares
Out[16]: [0, 1, 4, 9, 16]
```

What about generators? I'll use this custom generator_range function:

```
In [19]: def generator_range(n):
    ...:     x = 0
    ...:     while x < n:
    ...:         yield x
    ...:         x += 1
    ...:

In [20]: rng = generator_range(5)

In [21]: rng
Out[21]: <generator object generator_range at 0x7fac65d006e0>

In [22]: dir(rng)
Out[22]:
['__class__',
 '__delattr__',
 '__doc__',
 '__format__',
 '__getattribute__',
 '__hash__',
 '__init__',
 '__iter__',
 '__name__',
 '__new__',
 '__reduce__',
 '__reduce_ex__',
 '__repr__',
 '__setattr__',
 '__sizeof__',
 '__str__',
 '__subclasshook__',
 'close',
 'gi_code',
 'gi_frame',
 'gi_running',
 'next',
 'send',
 'throw']

In [23]: rng[3] # Nope
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-23-90eaee0d7e42> in <module>()
----> 1 rng[3]

TypeError: 'generator' object has no attribute '__getitem__'

In [24]: next(rng)
Out[24]: 0

In [25]: next(rng)
Out[25]: 1

In [26]: rng = generator_range(5) # Reset rng

In [27]: sum(rng)
Out[27]: 10

In [28]: next(rng) # The generator is exhausted after calling sum() on it
---------------------------------------------------------------------------
StopIteration                             Traceback (most recent call last)
<ipython-input-28-a4b881fa7ec6> in <module>()
----> 1 next(rng)

StopIteration:

In [29]:
```

The generator_range function returns a generator object. Generator objects have the `next()` method which, like any iterator, holds its present state.

To compare `xrange` to my custom `generator_range` function, it seems that, roughly: `xrange` returns an object is an iterable, which in turn returns an iterator, whereas `generator_range` returns a generator object which is an iterator.

Okay, enough with `xrange`.
