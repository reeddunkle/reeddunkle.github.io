---
layout: post
title: The Map Method
tags:
- Python
- Functional
---

The `map()` method is a way to apply the effect of a function on every element in an iterable. It iterates over the iterable, and passes each element in turn into the function you provide it. Each of these return values make up a new iterable, which `map()` returns.

A fact that I found useful about `map()`: The iterable it returns will always be the same size of the original.
