---
layout: post
title: Python's Iterables and Iterators
---

Python has iterables and iterations, both of which are invoked during iteration (or while iterating). This is one of those things that is confusing until it isn’t, and there isn’t a great way to narrow that gap. I cycle between, “Oh, I get it.” and “Wait, what?…”

There are strange logical differences between iterables and iterators, that demonstrate their interdependence on one another, but rather than explain one another they tend to confuse one another because they are so intimately tied.

The final layer of confusion I’ve had to unravel is that, for my day-to-day use of iteration, Python takes care of these two concepts (iterables and iterators) without requiring me to know what’s going on behind the scenes.

When I use a for-loop to iterate over a list, for example, conceptually I understand that I am iterating over a list…and that’s all I am really required to know.

**Diving In**:

When we create a list in Python...

```
the_list = [1, 2, 3]
```

We know that we can iterate over the list by invoking:

```
for element in the_list:
  print element
```

That has been all I’ve needed to know for most of the coding I’ve done up until this week. Experienced coders will say, “Reed’s a noob.” (True), and beginners will say “What else is there to understand about what’s going on?” (The point of this post)

It turns out that a list is iterable, but that it can’t be iterated on until it is  made into an iterator (for-loops do this for you), at which point it can be iterated on.

An iterator has the function `__next__()`, which retrieves and returns the next element in an iterable (beginning with the first element).

A for-loop takes the_list above, and calls `the_list.next()` over and over until it satisfies the parameters of the for-loop (in our example above that parameter is that you’ve reached the end of the list).

However, you can’t call `next()` on a list by default, you have to first turn it into an iterator. A list is an interable, meaning that it can be turned into an iterator, but it isn’t an iterator until you’ve turned it into one.

Confused yet?

Part of the confusing part of this all, for me at least, is that these concepts aren’t that complicated. It feels more complicated than it is. And we are accustomed to for-loops doing this work for us.

Hop into a Python repl, and I’ll show you the simple aspects of what a for-loop does, to demonstrate the differences between iterables and iterators.
>>> the_list = [1, 2, 3]
>>> the_list
[1, 2, 3]
>>>the_list.next()
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
AttributeError: 'list' object has no attribute 'next'

To make the_list into an iterator, on which you can call the .next() function, to retrieve the next element in the iterable, you call the iter() function on the iterable.

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

StopIteration is the Exception that is raised when .next() has already retrieved all of the elements in the iterable.

There are a couple of concepts here that are important for understanding the distinction between iterables and iterators. Forgive me for repeating myself a little now. I want to bring back the points I made earlier and try to demonstrate more examples of how these two concepts are different and interdependent.

How they are different:

Here is the distinction for you to refer to:
The iter() method returns an iterator of an iterable.

Above we had a container (a list) that has the ability to be made into an iterator. We called this iterable “the_list“. We then set the variable “list_that_can_be_iterated_on” to be an iterator of the iterable “the_list“. Then we called .next() on the iterator “list_that_can_be_iterated_on“, and each time it retrieved and returned the next element of the original iterable “the_list“.

This article calls iterators lazy, because they don’t do anything more than bookmark their position within an iterable, and when .next() is called on them, return the next element, and in turn bookmark that position.

To demonstrate this, I’ll first call .next() on an iterator, and then use a for-loop on the iterator to print out its elements. The first next() call will retrieve its first element, which means that the next time .next() is called on it, it will move to the second element and so on. When I use the for-loop, it will start at the second element, because .next() was already called once on the iterator.

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

Notice at the end of this that “the_list” has not been affected. It still contains a list of [1, 2, 3]. This is what allows us to call for-loops multiple times on the same iterables. To do that here, we could just keep re-entering “list_that_can_be_iterated_on = iter(the_list)“, and each time we would receive a fresh iterator of the_list, that starts at its first element.

Also, I did something above that is a bit confusing. I took “the_list” (an iterable) and stored it in “list_that_can_be_iterated_on” as an iterator (with the iter() method), and then I called it in a for-loop. I did this to show you in a familiar form (the for-loop) that after already having called .next() on the iterator, the for-loop would start at the second element (because we already called .next() on it once, outside of the for-loop).

I have glossed over the fact that normally when you call use a list (an iterable) in a for-loop, it both turns it into an iterator without showing us that it is doing this, and then iterates over the iterator, returning each element by calling .next() on the (invisible) iterator that it creates out of our original iterable.

Note: Iterators are lazy, but they still contain all of the information of the original iterable. To demonstrate this, type:

the_list = [1, 2, 3]
iterator_one = iter(the_list)
iterator_two = iter(iterator_one)

You can then call next() on iterator_one and iterator_two independently.

Apparently this is not what is happening when you use an object that is already an iterator in a for-loop like I did above. Otherwise, we should be able to call next() on “list_that_can_be_iterated_on” after the for-loop, and still retrieve the next element. But we can’t; it raises a StopIteration exception.

Let’s review the important concepts with custom classes:

A list is an example of an iterable. An iterable is defined as a object that has the __iter__ method, which returns an iterator of the iterable.

The custom iterator class:

class MyIterator(object):

  def __init__(self, iterable_that_is_made_iterator):
    self.iterable_that_is_made_iterator = iterable_that_is_made_iterator
    self.index = 0   # I assume we're already receiving an iterable

 ''' Below is the next() method we kept using above (Yes, I know this is a
 terrible comment for experienced coders, but I want to make it clear to
 beginners where I am using the concepts we used above.)
 '''
  def next(self):
    try:
      element = self.iterable_that_is_made_iterator[self.index]
      self.index += 1
      return element
    except IndexError:
      raise StopIteration

Back to the very first example in this post:

the_list = [1, 2, 3]

Remember that this doesn’t do anything yet:

the_list.next()

A list is an iterable, but it is not yet an iterator. Let’s use our custom MyIterator class to make it an iterator:

list_that_can_be_iterated_over = MyIterator(the_list)

This does the same thing as calling iter() on a list*. Now we can call the next() method on it and retrieve each element in turn.

*Note: This custom MyIterator class does not work with dictionaries, or necessarily any other types than lists. I don’t understand all of this completely. This post is as much for my sake as it is for yours. (This is where the __getitem__ method comes in.) It does, however, demonstrate how iterables and iterators differ from one another, and how they depend on one another, and in this case, how a list becomes an iterator and is iterated over with the next() method.

The last point that I want to demonstrate is how to reproduce the way a for-loop works to print out (for example) each element in a list.

the_list = [1, 2, 3]

for element in the_list:
  print element

Behind the scenes, this is what the for-loop does (and remember, MyIterator() is the same as the iter() method for our purposes):

the_list = [1, 2, 3]

# What the for-loop does:
iterator_list = iter(the_list)

while True:
  try:
    print iterator_L.next()
  except StopIteration:
    break

I will edit this post and try to make it cleaner. If this is confusing to you to whatever degree, check out the following explanations from other people, and see if they make things clearer. I tried to over-explain each step of how this works, but I worry that it does make it all feel more muddled than it needs to be.

Again, this is a tough problem, because it’s confusing up to the point when it isn’t. Hopefully my long explanation makes it easier for some to understand, but I suspect that more important than the right explanation is general exposure to the confusion, stretched out over time, flopping around in the confusion, reading any explanation that helps to whatever degree, and out of this will come some clarity.

Here are some other explanations, more thorough, but possibly more confusing:

http://nvie.com/posts/iterators-vs-generators/

http://stackoverflow.com/questions/9884132/what-exactly-are-pythons-iterator-iterable-and-iteration-protocols

http://stackoverflow.com/questions/32799980/what-exactly-does-iterable-mean-in-python
