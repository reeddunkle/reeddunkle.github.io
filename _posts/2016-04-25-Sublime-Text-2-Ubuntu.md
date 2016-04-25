---
layout: post
title: Sublime Text 2 On Ubuntu
---

**Note: I was getting this [annoying error](http://stackoverflow.com/questions/23165426/sublime-text-on-ubuntu-14-04-keeps-attempting-to-remove-it) which seems to happen because I am on Ubuntu 14.04. Apparently upgrading to Sublime Text 3 will fix it. I am now in the process of installing and setting up ST3. I’ll post about that I’m sure.**

  

It took me a bit of time to set up Sublime Text 2 (ST2), and it required a lot of troubleshooting (using Google), lots of resources which all had different suggestions, and generally some headache.
  
I hope this guide will make things simpler the next time I or anyone needs to install and set up Sublime Text 2.

**Specifications:** Ubuntu 14.04.4 LTS

## **Install Sublime Text 2**:
 - I followed [this guide](http://askubuntu.com/questions/172698/how-do-i-install-sublime-text-2-3) to install via the Package Manager (apt-get):  
    `$ sudo add-apt-repository ppa:webupd8team/sublime-text-2`  
    `$ sudo apt-get update`  
    `$ sudo apt-get install sublime-text`  

## **Set up Sublime Text 2**:
 - This requires about 30 minutes
 - I followed this [clean, well-written guide](https://blog.alexmaccaw.com/sublime-text). He wrote the guide for OSX, so I’ll translate the steps for Ubuntu:
  
#### **Open Sublime Text for the first time**:  
  1. Go to your terminal and enter `$ subl` to open Sublime.  
  2. Right click on the icon in the Launcher, and **Lock to Launcher**  
  3. Next, close the terminal that you used to open Sublime the first time. This will close Sublime.  
  4. Last, re-open Sublime from the Launcher, this time independent from any open Terminal.  
  (I do this because I find that as I open applications, and windows, and terminals, my environment quickly becomes messy and disorganized. I like to have my applications open independent of their terminals to make my life easier.)

#### **Open the editor’s console**:
  1. Go to [this website](https://packagecontrol.io/installation#st3). Select Sublime Text 2 at the top, and copy the entire block of code.
  2. Go back to Sublime, Press **ctrl+`** (control backtick)
  3. Paste the code and press Enter
    - The code at that website will update with every release.
  4. **Restart Sublime** (You may have to do this more than once as it updates or installs things. They will prompt you to restart it.)

#### **How to Install Packages:**
  1. Press **ctrl+shift+p**
  2. This opens a search box at the top of the Sublime page
  3. Type **Install Package**, and select “**Package Control: Install Package**” when it comes up
  4. Another search box will come up in place of the previous one
  5. From here you search for the different packages that you want. I installed the ones that the guide recommends. One or two of them might not have come up when I typed in the name. If so, I simply skipped them.

#### **Install Packages (and set them up)**:
In the second search box that I describe above, type the name of the package, and press Enter when it comes up.
  - **Theme – Soda**
  - **SideBarEnhancements**: (I feel like this one might not have come up for me…)
  - **AllAutocomplete**
  - **TrailingSpaces**: Very useful

#### **Configure your Settings**:
  1. In the menu bar along the **top of your screen** (hold **Alt** if it isn’t coming up), navigate to:
    **Preferences > Settings – User**
  2. This will bring up a document (in Sublime) that looks roughly like:
    `{ `
    `"font_size": 14,`
    `"ignored_packages":`
    `[`
    `"Vintage"`
    `]`
   ` }`
  3. I’m going to paste my **Settings – User** file here:
	`{`
	`“font_size”: 16,`
	`“ignored_packages”:`
	` [`
	`“Vintage”`
	`],`
	`“tab_size”: 4,`
	`“translate_tabs_to_spaces”: true,`
	`"theme”: “Soda Dark.sublime-theme”,`
	`“trim_trailing_white_space_on_save”: true`
 	`}`
 4. The “`theme`” above selects the **Theme – Soda** (I chose Dark version — replace Dark with Light for alternate)
 5. Adjust “`tab_size`” if needed

#### **Configure *more* settings**:
1. Navigate in Sublime’s menu bar to:
**Preferences > Package Settings > TrailingSpaces > Settings – User**
2. Add this:
        `{`
        `"trailing_spaces_include_current_line": false`
       ` }`

#### **Configure a few Key Bindings**:
1. Navigate in Sublime’s menu bar to:
        **Preferences > Key Bindings – User**
2. Here is my file:
        `[`
        `{ "keys": ["super+v"], "command": "paste_and_indent" },`
        `{ "keys": ["super+shift+v"], "command": "paste" },`
        `{ "keys": ["shift+space"], "command": "move_to", "args": {"to": "eol", "extend": false} }`
       ` ]`
      **Note**:
      - The first key binding replaces the **ctrl+v** function with “**Paste and Indent**“. This will adjust your **Paste** to automatically match the indentation of that in which you’re pasting.
      - The second key binding assigns your standard Paste function to ctrl+shift+v
      - The last key binding is one of my own making. Pressing **shift+space** will jump to the end of the line I’m on.  

	**Another Note**:
	- A built-in key binding that is really useful is **ctrl+m**. This will jump to the end of the current bracket in which you’re typing. 

#### **Don’t try to change Sublime’s icon**
 - The guide I was mentioned suggested doing this.
 I embarked upon that mission not knowing what I was getting myself into. It took me at least an hour to get it set up, and it wasn’t a big deal in the first place.
**Instead of coding for an hour+, I troubleshot changing icons in Ubuntu for an hour+**

#### **Set up a binary?**
- I don’t know what this means, honestly, but the guide that I followed suggests doing it.
- I can’t remember whether I actually did it.
- I think this allows you to open Sublime, and to open documents and folders in Sublime, by using the keyword “subl”.
- I think that this might work by default now, perhaps a change since the guide was written?

#### **I ignored all of the extra packages the guide recommends at the end.**
<br>
#### **A couple of things I figured out on my own:**
  - Set the language you’re coding in by navigating to:
    **View > Syntax**
  - It’s useful to open entire directories in Sublime

_Sources:_
[Guide I followed](https://blog.alexmaccaw.com/sublime-text)
[Set up end-of-line keybinding](http://stackoverflow.com/questions/14394598/move-to-end-of-line-without-end-key-in-sublime-text2)
[Discover ctrl+m shortcut](https://forum.sublimetext.com/t/jump-to-matching-bracket-addition/3593)

