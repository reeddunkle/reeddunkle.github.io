---
layout: post
title: Setting Up Learn.co on Ubuntu
---

When I first started on Learn.co, I was on Windows, and I used the Nitrous IDE, and they walk you through the set-up. I then used my girlfriend's iMac, and the set-up was easy, and again they walk you through it.

Today, with the help of [Ryan McNeely](http://students.learn.co/students/ryan-mcneely.html), I got it set up on Ubuntu.

**Specifications**: I'm on Ubuntu 14.04.4 LTS

**Here is what I had to do:**

#### **Install Ruby 2.2.3**
I followed [this tutorial](https://gorails.com/setup/ubuntu/14.04). I did the "Installing Ruby" section, and also the "Configuring Git" section.

#### **Configure Git**:
[See above]

#### **Install RVM**
I'm not sure how to take advantage of this yet, but Ryan recommended it, and I can see the advantage of having a Version Manager. I followed [this guide](http://www.webupd8.org/2014/11/how-to-install-rvm-ruby-version-manager.html).

Note: I realize now that in the first tutorial I used to install Ruby 2.2.3, he recommends choosing **rbenv** or **RVM**. I installed Ruby in step 1 using rbenv, and then I installed rvm per Ryan's suggestion. I don't know enough to know if this is going to cause any issues down the road.

#### **Install the learn gem**
This is the part that took the most troubleshooting for me.

- Enter this into the terminal, and wait for it to install:
`$ gem install learn-co`
- I then entered:
`$ learn help`
- And it said:
```
To connect with the Learn web application, you will need to
configure the Learn gem with an OAuth token. You can find yours
at the bottom of your profile page at:
https://learn.co/your-github-username.
Once you have it, please come back here and paste it in:
```

  - They want you to change "your-github-username" to your actual github username (this didn't occur to me at first...), which will take you to your Learn profile page.
    - I went to: [https://learn.co/reeddunkle](https://learn.co/reeddunkle)
  - All the way at the bottom of the page, in a tiny font, which is a color that almost blends in with the background color of the page, is your **OAuth token**.
  - Copy this string of characters, and paste it into your terminal (Ctrl+Shift+V, or middle mouse click), and press Enter. You should then see:
    `Authenticating...`
    `Commands:`
    ` learn [test] [options] # R...`
    ` learn directory # S...`
    ` learn doctor # C...`
    ` learn hello # V...`
    ` learn help [COMMAND] # D...`
    ` learn next [--editor=editor-binary] # O...`
    ` learn open [lesson-name] [--editor=editor-binary] # O...`
    ` learn reset # R...`
    ` learn save # S...`
    ` learn status # G...`
    ` learn submit [-m|--message "message"] [-t|--team @username @username2] # S...`
    ` learn version, -v, --version # D...`
    ` learn whoami # D...`  
  - Next, enter:
    `$ cd ~`
    `$ ls`
  - Now you should see a code directory listed. Inside the code directory is a labs directory, which you then use the same that you used the labs directory in Nitrous.
  You should be good to go!
