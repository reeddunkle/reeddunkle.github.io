---
layout: post
title: Closures - Part III
tags:
- Python
- Closures
---

### Part III - Diving In

Let's play around in the repl.

Feel free to call your variables anything else for convenience's sake. I just want to be clear what we're talking about at different moments, so I've been very explicit.

You'll notice I lose count of my line numbers. I'm playing around as we do this. You should too. I want to show you the things you can do with this.

You should try other things. This is only what I've found so far.

----

Because it offers tab-complete (and lots of other great features), I'm going to use [ipython](https://ipython.org/install.html)

```ipython
In [1]: def add(x, y):
   ...:     return lambda a, b: a+b+x+y
   ...: 

In [2]: add_4_and_3_to_a_number = add(4, 3)

In [3]: add_4_and_3_to_a_number
Out[3]: <function __main__.<lambda>>

In [4]: add_4_and_3_to_a_number.__  # Tab complete:
add_4_and_3_to_a_number.__call__          add_4_and_3_to_a_number.__hash__
add_4_and_3_to_a_number.__class__         add_4_and_3_to_a_number.__init__
add_4_and_3_to_a_number.__closure__       add_4_and_3_to_a_number.__module__
add_4_and_3_to_a_number.__code__          add_4_and_3_to_a_number.__name__
add_4_and_3_to_a_number.__defaults__      add_4_and_3_to_a_number.__new__
add_4_and_3_to_a_number.__delattr__       add_4_and_3_to_a_number.__reduce__
add_4_and_3_to_a_number.__dict__          add_4_and_3_to_a_number.__reduce_ex__
add_4_and_3_to_a_number.__doc__           add_4_and_3_to_a_number.__repr__
add_4_and_3_to_a_number.__format__        add_4_and_3_to_a_number.__setattr__
add_4_and_3_to_a_number.__get__           add_4_and_3_to_a_number.__sizeof__
add_4_and_3_to_a_number.__getattribute__  add_4_and_3_to_a_number.__str__
add_4_and_3_to_a_number.__globals__       add_4_and_3_to_a_number.__subclasshook__

In [4]: add_4_and_3_to_a_number.__closure__
Out[4]: 
(<cell at 0x7f5a664ada98: int object at 0xca8110>,
 <cell at 0x7f5a663c3b08: int object at 0xca8128>)

In [5]: closure_data = add_4_and_3_to_a_number.__closure__  # Store the above information in a variable

In [6]: closure_data  # Contains a tuple of two cells
Out[6]: 
(<cell at 0x7f5a664ada98: int object at 0xca8110>,  
 <cell at 0x7f5a663c3b08: int object at 0xca8128>)

In [10]: closure_data[0]  # Okay, so we can access each element in the tuple.
Out[10]: <cell at 0x7f5a664ada98: int object at 0xca8110>

In [11]: closure_data[1]  
Out[11]: <cell at 0x7f5a663c3b08: int object at 0xca8128>

In [13]: closure_data[1].cell_contents  # Nothings comes up with tab-complete for this unfortunately
Out[13]: 3

In [14]: closure_data[0].cell_contents
Out[14]: 4
```

Okay, I need to think about this in English.

We have our `add` function that takes two numbers and returns a function that takes two numbers and which returns the sum of all 4 numbers.

We set the variable `add_4_and_3_to_a_number`
