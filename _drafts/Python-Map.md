---
layout: post
title: The Map Method
tags:
- Python
- Functional
- Beginner to Intermediate
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

- Make a list of numbers: `NUMBERS = range(1, 100)`
- Run this `numbers` through your `my_map` function with an [anonymous function](http://reeddunkle.github.io/Python-Lambda-Closures/) which doubles each number
- Print the output

- Did it work? Here's [**my solution**](https://gist.github.com/reeddunkle/4ff0d639155b4f921d6b0c3a925a63e5)

### Exercise 8

- Take this list of strings:

```python
PICTURES = ["picture1", "georgi_paws_everywhere", "custom_wreath"]
```

- Map the extension `.jpg` on to the end of each of these strings

- Did you get it working? Here's [**my solution**](https://gist.github.com/reeddunkle/863db100ab5c0d96149d5dc4dc9c79f9)

### Exercise 9

- Make a new list of numbers: `numbers = range(4)`
- Take this list of strings:

```python
NAMES = ['Rebecca', 'Georgi', 'Reed', 'Thom Yorke']
```

- Use your `my_map` method to return a list of tuples, the first element of the tuple should be the number in `numbers`
- The second element in the tuple should be the name in `names` at the index of each number from `numbers`
- Does that make sense? An example tuple would be:
    - `(0, 'Rebecca')`

**Hints:**

- If using a lambda function confuses you, make a separate function
- Tuples are immutable, so you'll need to construct the tuple in that function (you can't `append()` elements onto a tuple

- Did you get it working? Here's [**my solution**](https://gist.github.com/reeddunkle/cd2bfb661fe1542681cc4f93a347937e)

----

### The built-in `map()` method

_map(function, iterable, ...)_

From the [documentation](https://docs.python.org/2/library/functions.html#map):

> Apply function to every item of iterable and return a list of the results. If additional iterable arguments are passed, function must take that many arguments and is applied to the items from all iterables in parallel. If one iterable is shorter than another it is assumed to be extended with None items. If function is None, the identity function is assumed; if there are multiple arguments, map() returns a list consisting of tuples containing the corresponding items from all iterables (a kind of transpose operation). The iterable arguments may be a sequence or any iterable object; the result is always a list.

To figure out what this does, I played around with it. For whatever reason, I started with this part:

> If function is None, the identity function is assumed; if there are multiple arguments, map() returns a list consisting of tuples containing the corresponding items from all iterables (a kind of transpose operation). The iterable arguments may be a sequence or any iterable object; the result is always a list.


By passing in `None` as the function value, it says "the identity function is assumed". I don't know what that means, but I tried giving it `None` as the function, and giving it both `NUMBERS` and `NAMES` in the iterable list, and look what it did:

```bash
>>> NUMBERS = range(4)
>>> NAMES = ['Rebecca', 'Georgi', 'Reed', 'Thom Yorke']
>>>
>>> results = map(None, NUMBERS, NAMES)  # None as the function, and added both iterables
>>> print results
[(0, 'Rebecca'), (1, 'Georgi'), (2, 'Reed'), (3, 'Thom Yorke')]
>>> 
```

Well, that's cool. Our custom map function certainly won't do that. That takes care of Exercise 9. This seems like a really easy way to combine lists.

Now I'm not sure about that first sentence:

> If additional iterable arguments are passed, function must take that many arguments and is applied to the items from all iterables in parallel.

I tried this:

```bash
>>> NUMBERS = range(4)
>>> NAMES = ['Rebecca', 'Georgi', 'Reed', 'Thom Yorke']
>>>
>>> results = map(lambda x, y: (x, y), NUMBERS, NAMES)
>>> print results
[(0, 'Rebecca'), (1, 'Georgi'), (2, 'Reed'), (3, 'Thom Yorke')]
```

Then I tried this:

```bash
>>> NUMBERS = range(1, 5)  # I changed this to be 1-4
>>> NAMES = ['Rebecca', 'Georgi', 'Reed', 'Thom Yorke']
>>> results = map(lambda x, y: x*y, NUMBERS, NAMES)  # I changed this to multiply the values
>>> print results
['Rebecca', 'GeorgiGeorgi', 'ReedReedReed', 'Thom YorkeThom YorkeThom YorkeThom Yorke']
```

This means that `map()` takes the first elements from both iterables, and feeds them into the function.

`1 * 'Rebecca' = 'Rebecca'`

Then it takes the second element from both iterables and feeds them into the function:

`2 * 'Georgi' = 'GeorgiGeorgi'`

Then:

`3 * 'Reed' = 'ReedReedReed'`

And then lots of Thom Yorke.

Before writing this I didn't know that `map()` could take `None` as an argument for the function, or that it could take multiple iterables and work through them in parallel.

Thank you for reading this, and learning with me.


