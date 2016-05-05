---
layout: post
title: Python List Comprehension
tags:
- Python
- Functional
---

List comprehension is an easy way to define lists in Python. It is often an alternative to the lambda function, and is considered a more Pythonic way to create the functional methods [`filter()`](http://reeddunkle.github.io/Python-Filter/), [`map()`](http://reeddunkle.github.io/Python-Map/), and `reduce()`.

From the [docs](https://docs.python.org/2/tutorial/datastructures.html#list-comprehensions):

> List comprehensions provide a concise way to create lists. Common applications are to make new lists where each element is the result of some operations applied to each member of another sequence or iterable, or to create a subsequence of those elements that satisfy a certain condition.

----

### Example 1: How it works

It's easy, honestly. You just have to adjust the way that you express your logic. List comprehensions read a little more like English.

Here's an example. I'm stealing the first example from the docs.

Say we want to do this:

```bash
>>> numbers = range(10)
>>> squares = []
>>> for x in numbers:
...     squares.append(x**2)
... 
>>> squares
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

To create this list using **list comprehension**, we could instead do this:

```bash
>>> numbers = range(10)
>>> squares2 = [x**2 for x in numbers]
>>> squares2
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

In English, this reads a bit like:

- Set `squares2` to a list
- Fill it with `x**2` for every x in the range of 0 to 9

To compare this to a traditional function:

- In the first part of the list comprehension, you put the operation(s) you want to do, whose return values will populate the list you're making
- The second part should iterate over the iterable from which you want to create your new list

#### Map as list comprehension

This is exactly what we do with the `map()` function. We take a list `numbers`, we map an effect across every item in that list -- here the effect is raising each element to the power of 2 -- and we make a new list of the return values of doing this.

The difference is that, instead of passing `map` a function that raises each element to the power of 2, and an iterable from which to feed into the function, we tell the list comprehension what to do with elements it receives, and we define a for-loop to do the iterating and value passing.

List comphrension has been deemed a more Pythonic way of mapping an effect onto elements in a list than the built-in `map()` function.

For my purpose, I'm happy to abide by best practices regardless of the philosophy, but if you care about the debate, I believe that [people](https://en.wikipedia.org/wiki/Benevolent_dictator_for_life) prefer list comprehensions to functional methods for two reasons:

1. List comprehensions read more like math/logic expressions
2. They also read a little more like English

----

### Example 2

I'm going to switch to strings for a change of pace.

Let's say we have a block of text:

```bash
text = "Hello, my name is Reed. I am 28 years old. I am a hack-hack-hackin away at the Recurse Center in NYC. I play around in Python and I like words and vinegar."
```

I haven't covered all of these functions. I will, or should, but for now just follow this.

```bash
text_list = text.split()  # It defaults to splitting on spaces
>>> text_list
['Hello,', 'my', 'name', 'is', 'Reed.', 'I', 'am', '28', 'years', 'old.', 'I', 'am', 'a', 'hack-hack-hackin', 'away', 'at', 'the', 'Recurse', 'Center', 'in', 'NYC.', 'I', 'play', 'around', 'in', 'Python', 'and', 'I', 'like', 'words', 'and', 'vinegar.']
```

Now we have a list of the words from the original text, which I stored in a variable called `text_list`. Now we can use this in some examples of list comprehensions.

#### Filter as a list comprehension

Let's say we want to filter this list and only look at words longer than 4 characters. Continuing from the code above:

```bash
>>> long_words = [words for words in text_list if len(words) > 4]  # Notice our boolean expression
>>> long_words
['Hello,', 'Reed.', 'years', 'hack-hack-hackin', 'Recurse', 'Center', 'around', 'Python', 'words', 'vinegar.']
```

In English:

- Make `long_words` a list of `words for` the `words in text_list` if the length of the words is greater than 4.

What if we want to look at frequency instead of size, the words that appear at least twice in the text.

```bash
>>> freq_words = [words for words in text if text.count(words) >= 2]
>>> freq_words
['I', 'am', 'I', 'am', 'in', 'I', 'in', 'and', 'I', 'and']
```

----

I think that exercises are a good way to get a feel for these things (I suppose this is obvious, as I've been assigning exercises without any preface)

If you agree:

### Exercise 8.2:

Take the list from before:

```python
FILES = ["picture1", "georgi_paws_everywhere", "custom_wreath"]
```

Map the ".jpg" extension onto each string using a list comprehension.

[**SOLUTION**](https://gist.github.com/reeddunkle/46cbb8a6a61d9b9ae219177c39c60575)

----

### Exercise 9.2:

Take the lists from before:

```python
NUMBERS = range(4)
NAMES = ['Rebecca', 'Georgi', 'Reed', 'Thom Yorke']
```

Map them into a tuple using a list comprehension.

[**HACKY SOLUTION**](https://gist.github.com/reeddunkle/bd9671b802ad53656dcccf34673ed427)
