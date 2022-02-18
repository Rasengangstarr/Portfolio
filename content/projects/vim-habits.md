---
title: "Vim Wiki Habits"
date: 2022-02-18T20:12:30Z
draft: false 
summary: "A habit tracker plugin for Vim"
---

[Github Page](https://github.com/Rasengangstarr/vimwikihabits).

A few months back, I decided I was going to finally force myself to learn [Vim](https://www.vim.org/). 

I ended up building a 60% keyboard with snap in switches (which was a fun project in and of itself), which crucially didn't have any arrow keys, forcing me to learn vim's hjkl control system.

![My Keyboard](/images/mykeyboard.jpg)

Anyway, somewhere down that rabbit hole, I discovered [Vim Wiki](https://github.com/vimwiki/vimwiki) and Vimscript.

# Vim Wiki

Vim Wiki is a vim extension that allows you to create markdown documents that are rendered in vim. Obviously since vim runs in a terminal session that doesn't mean much, but it does allow you to do some cool things like bullet point lists, checklists and impressively create hyperlinks between documents.

Bundled with Vim Wiki is a diary feature. Using hotkeys, you can create diary entries for the current day, and generate an index page to navigate them.

# Vimscript

Vimscript the scripting (duh) language used to create vim plugins. It's quite limited, and you have to jump through some hoops to get anything done, but I spent some time working through [This set of tutorials](https://learnvimscriptthehardway.stevelosh.com/) to get to grips with it.

# My Project

I figured that a cool (and potentially actually useful) extension to the Diary in VimWiki would be a habit tracker.

I wanted to be able to define a set of habits, and have them pasted into Diary pages as a list of checkboxes. I then wanted to take those filled in checkbox lists and use them to generate a report of what habits had and hadn't been followed.

The code (which you can find on [Github](https://github.com/Rasengangstarr/vimwikihabits)) is an utter mess, but it just about works.

# How its used

First, navigate to your vimwiki diary directory, and create a new file called habits.vim. Inside it, enter your habits as a new line seperated list:

![screenshot-1635963144](/images/vimwikihabits1.png)

Save this file, then enter one of your diary entries, (or create one using the vimwiki diary shortcuts:

![screenshot-1635963379](/images/vimwikihabits2.png)

Press <esc> :VimWikiHabitsInsert

![screenshot-1635963519](/images/vimwikihabits3.png)

  
This should paste your habits as a todo list in the diary entry - check or uncheck them by pressing <ctrl>+<space> over the boxes.
  
Then you can simply press <esc> :VimWikiHabitsGenerate to view the habit tracker at the bottom of the screen
  
  ![screenshot-1635963495](/images/vimwikihabits4.png)

# Further work

Having gotten the vim plugin bug, I made two further plugins:

* [Vascii](https://github.com/Rasengangstarr/vascii), which utilizes the jp2a bash program to render an image file as ascii art directly into a document within vim
* [Vimcats](https://github.com/Rasengangstarr/Vimcats), which takes an image from this [Random cat image API](https://cataas.com/cat) and dumps it into a document within vim

# Conclusions

I've not done much with Vimscript since these projects, but I still use Vim every day. I'm currently editing this document with the Vim extension for Visual Studio Code, I use a Vim plugin for Visual Studio when I'm developing C# on Windows at work, and being able to quickly edit files on my home computer without leaving the terminal is awesome.

It takes a while to learn Vim's keys, but once you get the hang of it everything becomes easier, and using the 'standard' way of editing files feels like a huge step backwards.

My biggest problem now is accidentally typing :w in the middle of text files when I'm using notepad++ at work.

