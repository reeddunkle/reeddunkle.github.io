---
layout: post
title: Python Generators
tags:
- Python
- Generators
---

When I first heard about generators, it was in context of Python2, and the difference between **range()** and **xrange()**, `xrange()` being a generator function. In Python3, I learned, the original `range()` was removed, and `xrange()` was renamed `range()`. If you look up generators in Python, I can almost guarantee this will be the example you'll see.

Quick Introduction
----

I'm going to briefly cover the stuff you may have already heard, and then move onto more interesting things.

If you haven't used Python3 much, then you're accustomed to this:

```
>>> a = range(5)
>>> b = xrange(5)
>>> a
[0, 1, 2, 3, 4]
>>> b
xrange(5)
>>> type(a)
<type 'list'>
>>> type(b)
<type 'xrange'>
>>>
```

As you can see, the original `range()` function returns a list -- which means that the entire list is held in memory -- and on the other hand, `xrange()` is this other thing. It’s called a generator, and it brings one element at a time into memory as needed.

I didn’t catch on to this at first, but we commonly use lists in a few different ways, and the difference between these have different memory demands. One way we use lists is when part of our design is to edit and manipulate parts of the list, such as rearranging or sorting, and when we need to refer to the list throughout our program.

In this case it makes sense to hold the entire list in memory, and work with it until we’re done with it.

Another common way we use lists, however, is to build or accumulate a list whose elements we only need to access once for the purposes of iterating. We can save a lot of memory usage in these cases, if we can get away with only bringing one element into memory at a time.

Python’s `range()` is an example of this. We use it to do something a certain amount of times. Or we use it to quickly produce a sequence of numbers that we can refer to one at a time -- for indexing, or for filtering/reducing/mapping.

In these cases, we don’t require the entire list to be held in memory at the same time. We care about iterating through the elements, one at a time from start to finish. Once we’ve used that element we’re done with it until the next time we want to iterate through them.

I got to this point without too much confusion or objection. But it took me a while to move past this theory into the practical implications:

1. Should I always use a generator instead of a list when I can?
2. When is it safe to use a generator instead of a list? Maybe I should just stick with lists, and then [if I find that my program is running too slowly, then and only then should I go back and worry about tuning my code.](http://stackoverflow.com/questions/47789/generator-expressions-vs-list-comprehension)
3. What does it mean to “only bring one element into memory at a time”? Can/Should I make my own generators?

Practical Implications
----

The key to understand is that we want to use the container solely for the purpose of iteration. We want to go through the elements one at a time. With `range()`, we want to iterate over a range of numbers one at a time. Maybe we have a list of students’ names, and we want to reference them one at a time.

The core idea is that for this purpose we want to iterate over the list. We have a container of values, and for our current goal we only care about accessing one value at a time.

I avoided using generators for months until I realized this simple difference. Yes, we should use generators whenever we can. It only makes sense. Embrace this technology.

That answers questions 1 and 2 above, so now to 3.

Generator Expressions
----

If you're like me, you use [list comprehensions](http://reeddunkle.github.io/Python-List-Comprehension/) all over the place. If you change the brackets `[]` into parenthesis `()`, your list comprehension becomes a generator expression.


```
>>> a = [x**2 for x in range(5)]
>>> a
[0, 1, 4, 9, 16]
>>> b = (x**2 for x in range(5))
>>> b
<generator object <genexpr> at 0x7f4dc6089780>
>>>
>>> a[3]
9
>>>
>>> b[3]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'generator' object has no attribute '__getitem__'
>>>
```

When might this be useful? In [this](http://exercism.io/exercises/python/difference-of-squares/readme) exercism.io problem, you have to make a function that returns the sum of squares of a range. The code above calculates the squares of a range. For the exercism problem, you need to add these numbers together, so you call `sum()` on them:

```
>>> sum([x**2 for x in xrange(5)])
30
>>> sum((x**2 for x in xrange(5)))
30
>>>
```

Notice that to make this calculation, you didn't care about referencing the squares. Calculating the value of each number squared was just a temporary step necessary to ultimately sum these values. To calculate the sum, the computer had to reference each of these values only once.

You should use a generator for this.

**Note**:
If you're using a generator like `xrange` (or `range` in python3), then you don't need the parenthesis. Instead of this...

```
sum((x**2 for x in xrange(5)))
```

...you can do this:

```
sum(x**2 for x in xrange(5))
```

I don't know if this difference preserves anything but your eyes.

DIY Generators
----

> In practice (how you think about them), generators *are* functions, but with the twist that they're resumable.
> -Guido

For [this](https://www.hackerrank.com/challenges/map-and-lambda-expression) Hacker Rank problem, they want us to calculate the cubes of the first N fibonacci numbers.

I'm going to do it in python3.

Using generators, we could do this a couple of different ways. This way:

1. Makes a generator that gathers the first `n` fibonacci numbers
2. Uses `map()` and `lambda` to generate the cubes
3. In python3, `map()` returns a generator, so I use `list()` to convert it to a list. In python2, map returns a list

```python
def fibs(n):
    a, b = 0, 1
    i = 0
    while i < n:
        yield a
        a, b = b, a + b
        i += 1

if __name__ == '__main__':
    n = int(input())
    answer = map(lambda x: x**3, fibs(n))

    print(list(answer))
```

You could critique my using a generator at all in this challenge, since the final output must be a list anyways. That's definitely fair.


I like the generator approach, however, because it makes my code efficiently re-usable. Only at the end do I bring the full list into memory to satisfy the requirement of this particular challenge.

**Note**:
You can always call `list()` on a generator to bring the full sequence into memory and store it as a list.

Exercises
----

### Exercise 12

- The next time you make code that does this...

```python
def make_new_list(things)
    temporary_list = []
    for thing in things:
        temporary_list.append(do_something(thing))

    return temporary_list
```

...instead do this:

```python
def make_generator(things)
    for thing in things:
        yield do_something(thing)
```

### Exercise 13

For [this](http://exercism.io/exercises/python/run-length-encoding/readme) exercism problem, I made a solution that uses generators for the encoding and decoding. See if you can too!

[**MY SOLUTION**](http://exercism.io/submissions/f769de6ff62141eda728e93e45a1df9d)
