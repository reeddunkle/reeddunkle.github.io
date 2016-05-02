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

```bash
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

In [13]: closure_data[1].cell_contents  
Out[13]: 3

In [14]: closure_data[0].cell_contents
Out[14]: 4

In [23]: cell_1 = closure_data[0]  # To check for tab-complete options

In [24]: cell_1.cell_contents  # Nope, that's it.
```

Okay, I need to think about this in English.

We have our `add` function that takes two numbers and returns a function that takes two numbers and which returns the sum of all 4 numbers.

We set the variable `add_4_and_3_to_a_number` to the function returned by `add(4, 3)` (which is then: `lambda: a, b: a+b+4+3`

`add_4_and_3_to_a_number` has a method `__closure__` which returns this tuple of cells.

We store that tuple in a variable `closure_data` for easy access.

At the end I store an individual cell in a variable called `cell_1`, and can call `cell_contents` on that, which returns `4`.

This means that the `cell_contents` of these cells represents the value I originally gave the `add` function when I called it at the time of storing it the variable `add_4_and_3_to_a_number`

Okay, so _that's_ how the lambda knows what to refer to in this instance of `add`.

What else can we come up with? Continuing from above...

```ipython
In [27]: add_4_and_3_to_a_number.__
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

In [27]: add_4_and_3_to_a_number.__code__
Out[27]: <code object <lambda> at 0x7f5a66390630, file "<ipython-input-1-ad84dc0591c0>", line 2>

In [28]: code = add_4_and_3_to_a_number.__code__  # Store this to a variable for easy access

In [29]: code.__
code.__class__         code.__ge__            code.__lt__            code.__setattr__
code.__cmp__           code.__getattribute__  code.__ne__            code.__sizeof__
code.__delattr__       code.__gt__            code.__new__           code.__str__
code.__doc__           code.__hash__          code.__reduce__        code.__subclasshook__
code.__eq__            code.__init__          code.__reduce_ex__     
code.__format__        code.__le__            code.__repr__          

In [29]: code.
code.co_argcount     code.co_filename     code.co_lnotab       code.co_stacksize
code.co_cellvars     code.co_firstlineno  code.co_name         code.co_varnames
code.co_code         code.co_flags        code.co_names        
code.co_consts       code.co_freevars     code.co_nlocals      

In [29]: code.co_argcount
Out[29]: 2

In [33]: code.co_filename
Out[33]: '<ipython-input-1-ad84dc0591c0>'

In [34]: code.co_freevars
Out[34]: ('x', 'y')

In [35]: 


