---
layout: post
title: Upgrading to Sublime Text 3
tags:
- Sublime Text 2
- Sublime Text 3
---

I kept having this [error message](http://stackoverflow.com/questions/23165426/sublime-text-on-ubuntu-14-04-keeps-attempting-to-remove-it) come up on ST2, and it seems the solution was to install ST3.

I followed [this guide](http://hswolff.com/upgrading-to-sublime-text-3/) for the installation. I mostly went back to the same packages, however, that I installed in ST2. They were all available, and [my instructions](http://reeddunkle.github.io/Sublime-Text-2-Ubuntu/) for setting those up still works.

Installing **Theme – Soda** is the same process, but it is set up differently in the settings file.

I’ll post my **User – Settings** file here:

```
{
“font_size”: 13,
“ignored_packages”:
[
“Vintage”
],
“soda_classic_tabs”: true,
“soda_folder_icons”: true,
“tab_size”: 4,
“theme”: “Soda Dark 3.sublime-theme”,
“translate_tabs_to_spaces”: true,
“trim_trailing_white_space_on_save”: true
}
```
