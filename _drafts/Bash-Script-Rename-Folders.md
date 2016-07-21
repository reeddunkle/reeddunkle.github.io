---
layout: post
title: Bash Script - Renaming multiple folders
tags:
- Bash
---

I saw an obvious way to clean up my `hacker_rank` directory, and jumped into it. I had a series of directories from the [Hacker Rank Python exercises](https://www.hackerrank.com/domains/python/py-introduction) in my root directory called `collections_OrderedDict`, `collections_defaultdict`, etc. And another series called `itertools_groupby`, `itertools_combinations`, etc.

I hadn't specified which language at this point, but thought this would fit in with the clean up. In the root `hacker_rank` directory, I made `python_collections`, `python_itertools`, and `python_functionals` directories, mirroring Hacker Rank's categories.

I ran `mv collections_* python_collections` (and the same for `itertools_*`), which cleaned up my root directory a lot. Another abstraction layer would be to make a `python` directory, but right now I actually only do the Python exercises.

So then inside `hacker_rank/python_collections/` I had a series of directories named `collections_...`, which was now redundant.

I messaged [Leo](https://github.com/Leockard), and asked him to help me come up with a bash script to rename all of these in one swoop.

This is what he came up with:

```
find . -maxdepth 1 -name 'collections*' -exec sh -c 'mv {} `echo {} | cut -d _ -f2`' \;
```

- `find` all files on this directory (`.`)
- recursing until a max depth of 1
- whose name matches `'collections*'`
- on them `exec` the command `sh -c`
- which calls `sh` on the string passed as argument: 'mv {} `echo {} | cut -d _ -f2`'
- which in turn says "call mv on the matched file (`{}`) and change its name to whatever is inside the backticks
- which is the matched name (`echo {}`)
- which is then `cut` by the delimiter(`-d`) `_`
- and from that cut, you only return the second field: `-f2`

Leo says:
"The ending `\;` is a `find` thing, I think it needs it every time you use `-exec`"
