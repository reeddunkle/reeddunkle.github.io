---
layout: post
title: Python's Iterables and Iterators
tags:
- Python knowledge
- Python 2.7
---

Python has iterables and iterators, both of which are invoked during iteration (or while iterating). This is one of those things that is confusing until it isn’t, and there isn’t a great way to narrow that gap.

There are strange logical differences between iterables and iterators, which demonstrate their interdependence on one another. Rather than explain one another, however, they tend to confuse one another because both concepts are so intimately tied.

The final layer of confusion I’ve had to unravel is that, for my day-to-day use of iteration, Python takes care of these two concepts (iterables and iterators) without requiring me to know what’s going on behind the scenes.

When I use a for-loop to iterate over a list, for example, conceptually I understand that I am iterating over a list…and that’s all I am really required to know.

**Specifications**: I'm on Ubuntu 14.04 LTS, using Python 2.7.6

#### Diving In:

When we create a list in Python...

```python
the_list = [1, 2, 3]
```

We know that we can iterate over the list like this:

```python
for element in the_list:
    print element
```

That has been all I’ve needed to know for most of the coding I’ve done up until this week.

It turns out that a list is iterable, but that it can’t be iterated over until it is  made into an iterator (for-loops do this for you), at which point it can be iterated on.

An iterator has the function `__next__()`, which retrieves and returns the next element in an iterable (beginning with the first element).

A for-loop takes `the_list` above, and calls `the_list.next()` over and over until it satisfies the parameters of the for-loop (in our example above, that parameter is that you’ve reached the end of the list).

However, you can’t call `next()` on a list by default, you have to first get an iterator from the list, and on that iterator you can call `next()` to retrieve its elements.

I'll say that same thing in a slightly different way, to try to clear up the confusion:

A list is an iterable, which means that you can get an iterator from it (an iterable can return an iterator). An iterator has the `next()` method, which retrieves its elements one at a time, which is how we are accustomed to iterating over containers.

Confused yet?

Part of the confusing part of this all, for me at least, is that these concepts aren’t that complicated. It feels more complicated than it is. And we are accustomed to for-loops doing this work for us.

Hop into a Python repl, and I’ll show you the simple aspects of what a for-loop does, to demonstrate the differences between iterables and iterators.

```
python
Python 2.7.6 (default, Jun 22 2015, 17:58:13) 
[GCC 4.8.2] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> the_list = [1, 2, 3]
>>> the_list
[1, 2, 3]
>>> the_list.next()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'list' object has no attribute 'next'
>>>
```

To get an iterator from `the_list`, on which you can call the `next()` function, you call the `iter()` function on the iterable. `iter(the_list)` will return an iterator of `the_list`.
Continuing from above:

```
...
>>> list_that_can_be_iterated_on = iter(the_list)
>>> list_that_can_be_iterated_on.next()
1
>>> list_that_can_be_iterated_on.next()
2
>>> list_that_can_be_iterated_on.next()
3
>>> list_that_can_be_iterated_on.next()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
>>>
```

StopIteration is the Exception that is raised when `next()` has already retrieved the last element from the iterator (which has the same elements of the original iterable).

There are a couple of concepts here that are important for understanding the distinction between iterables and iterators. Forgive me for repeating myself a little. I want to bring back the points I made earlier and try to demonstrate more examples of how these two concepts are different and interdependent.

#### How they are different:

Remember:
The `iter()` method returns an iterator from an iterable.

Above we had a container (a list) that has the ability to return an iterator of itself. We called this iterable `the_list`. We then set the variable `list_that_can_be_iterated_on`” to be an iterator of the iterable `the_list`. Then we called `next()` on the iterator `list_that_can_be_iterated_on`, and each time it retrieved and returned the next element of the original iterable `the_list`.

[This article](http://nvie.com/posts/iterators-vs-generators/) calls iterators lazy, because they don’t do anything more than bookmark their position, and when `next()` is called on them, they return the next element, and in turn bookmark _that_ position.

To demonstrate this, I’ll first call `next()` on an iterator, and then use a for-loop on the iterator to print out its elements. The first `next()` call will retrieve its first element, which means that the next time `next()` is called on it, it will move to the second element and so on. When I use the for-loop, it will start at the second element, because `next()` was already called once on the iterator.

```
>>> the_list = [1, 2, 3]
>>> list_that_can_be_iterated_on = iter(the_list)
>>> list_that_can_be_iterated_on.next()
1
>>> for element in list_that_can_be_iterated_on:
...        print element
...
2
3
>>> the_list
[1, 2, 3]
```

Notice at the end of this that `the_list` has not been affected. It still contains a list of `[1, 2, 3]`. This is what allows us to call for-loops multiple times on the same iterables. To do that here, we could just keep re-entering this...

```
list_that_can_be_iterated_on = iter(the_list)
```

And each time we would receive a fresh iterator from `the_list`, that starts at its first element.

Also, I did something above that may be confusing. I took `the_list` (an iterable) and using the `iter()` method, I returned a iterator of `the_list` and stored it in `list_that_can_be_iterated_on`, and _then_ I called it in a for-loop. I did this to show you in a familiar form (the for-loop) that after already having called `next()` on the iterator, the for-loop would start at the _second_ element (because we already called `next()` on it once, outside of the for-loop).

I have glossed over the fact that _normally_ when you use a list (an iterable) in a for-loop, it gets an iterator from it without showing us that it is doing this, and then iterates over that iterator, returning each element by calling `next()` on the (invisible) iterator that it created out of our original iterable.

Apparently when you use an object that is already an iterator in a for-loop like I did above, it does not produce a fresh iterator from it. Otherwise, we would be able to call `next()` on `list_that_can_be_iterated_on` _after_ the for-loop, and still retrieve the next element. But we can’t; it raises a `StopIteration` exception.

#### Reviewing the important concepts with custom classes:

A list is an example of an iterable. An iterable is defined as a object that has the `__iter__` method, which returns an iterator of itself.

The custom iterator class:

```python
class MyIterator(object):

    def __init__(self, iterable_that_is_made_iterator):
        self.iterable_that_is_made_iterator = iterable_that_is_made_iterator
        self.index = 0   # I assume we're already receiving an iterable

    ''' Below is the next() method we kept using above. I want to make it clear
    where I am using the concepts we used above.
    '''
    def next(self):
        try:
            element = self.iterable_that_is_made_iterator[self.index]
            self.index += 1
            return element
        except IndexError:
            raise StopIteration
```

Back to the very first example in this post:

```
the_list = [1, 2, 3]
```

Remember that this doesn’t do anything yet:

```
the_list.next()
```

Let’s use our custom MyIterator class to get an iterator from it:

```
list_that_can_be_iterated_over = MyIterator(the_list)
```

**This does the same thing as calling `iter()` on a list**. Now we can call the `next()` method on `list_that_can_be_iterated_over` and retrieve each element in turn.

**Note:** This custom MyIterator class does not work with dictionaries, or necessarily any other types than lists. Each iterable has its own way of accessing its elements. I think that this is where the [__getitem__](http://stackoverflow.com/questions/9884132/what-exactly-are-pythons-iterator-iterable-and-iteration-protocols) method comes in. This MyIterator class does, however, demonstrate how iterables and iterators differ from one another, and how they depend on one another, and in this case, how a list generates an iterator that can then be iterated over with the `next()` method.

The last point that I want to demonstrate is how to reproduce the way a for-loop works to print out (for example) each element in a list.

```python
the_list = [1, 2, 3]

for element in the_list:
    print element
```

Behind the scenes, this is what the for-loop does:

```python
the_list = [1, 2, 3]

# What the for-loop does:
iterator_list = iter(the_list)  # Generate an iterator from the iterable

while True:
      try:
          print iterator_list.next()
      except StopIteration:
          break
```

I will continue to edit this post and try to make it cleaner and clearer. If this is confusing to you to whatever degree, check out the following explanations from other people, and see if they make things clearer. I have tried my own way of explaining this, but I worry that I've made it all feel more muddled than it needs to be.

I hope that my long explanation makes it easier for some to understand. I suspect, however, that what's more important than the perfect explanation is general exposure to the confusion stretched out over time, flopping around in the confusion, and reading any explanation that helps to whatever degree, and from there will come understanding.

Here are some other explanations, more thorough, but possibly more confusing:

<http://nvie.com/posts/iterators-vs-generators/>

<http://stackoverflow.com/questions/9884132/what-exactly-are-pythons-iterator-iterable-and-iteration-protocols>

<http://stackoverflow.com/questions/32799980/what-exactly-does-iterable-mean-in-python>
