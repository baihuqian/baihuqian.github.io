---
title: VIM Cheetsheet
description: This is a quick-but-dirty cheet sheet of VIM.
tags:
  - Linux
---

# Normal Mode
Note: Prefix a cursor movement command with a number to repeat it. For example, 4j moves down 4 lines.

* h - move left
* j - move down
* k - move up
* l - move right

--------

* w - jump to **start** of words (*punctuation* considered words)
* e - jump to end of words (*punctuation* considered words)
* b - jump backward to start of words (*punctuation* considered words)
* W - jump to **start** of words (*spaces* separated words)
* E - jump to end of words (*spaces* separated words)
* B - jump backward to start of words (*spaces* separated words)

-------

* 0 - start of line
* $ - end of line
* ^ - first non-blank character of line

------

* gg - go to start of file
* G - go to end of file
* 5G - go to line 5

------

* % - go to matching parenthesis or bracket

------
* za - fold and unfold code
* :e - reload the opened file


# Insert Mode

* i - start insert mode at cursor
* I - insert at the beginning of the line
* a - append after the cursor
* A - append at the end of the line, after the last character (useful to insert newline)
* o - append a blank line below current line and enter Insert mode
* O - prepend a blank line above current line and enter Insert mode

* Esc - exit insert mode

------

Useful combination:

* ea - append at end of word


# Editing
* r - replace a single character, rx replace current with x
* ~ - toggle case
* J - join line below to the current one
* cc - remove (change) an entire line, 2cc to remove (change) next 2 lines
* cw - remove (change) to the end of word
* c$ - remove (change) to the end of line
* s - delete (substitute) character at cursor
* S - delete (substitute) line at cursor (same as cc)
* u - undo
* ctrl + r - redo
* . - repeat last command
* \>\> - shift right
* << - shift left

## Cut and Paste

* yy - copy (yank) a line, 2yy to copy (yank) next 2 lines
* yw - copy (yank) word
* y$ - copy (yank) to end of line
* p - paste after cursor
* P - paste before cursor
* dd - cut a line (can be used for deleting a line too)
* dw - cut the current word
* x - cut current character
* xp - transpose two letters (delete and paste, technically)


# Visual mode selection

* v - start visual mode, mark lines, then do command
* V - start Linewise visual mode
* Ctrl + v - start visual block mode
* o - move to other end of marked area
* O - move to other corner of block
* aw - mark a word
* ab - a () block (with braces)
* aB - a {} block (with brackets)
* ib - content inside the () block
* iB - inner {} block
* Esc - exit visual mode

##  Visual mode operation
* \> - shift right
* < - shift left
* y - yank (copy) marked text
* d - delete marked text
* ~ - switch case

# Exiting

* :w - write (save) the file, but don't exit
* :wq - write (save) and quit
* :q - quit (fails if anything has changed)
* :q! - quit and throw away changes

# Search/Replace

* /pattern - search for pattern
* ?pattern - search backward for pattern
* n - next search result in same direction
* N - next search result in opposite direction
* :%s/old/new/g - replace all old with new throughout file
* :%s/old/new/gc - replace all old with new throughout file with confirmations

#Working with multiple files

* :e filename - Edit a file in a new buffer
* :bnext (or :bn) - go to next buffer
* :bprev (of :bp) - go to previous buffer
* :bd - delete a buffer (close a file)
* :sp filename - Open a file in a new buffer and split window

-------

ctrl + w works with windows:

* (ctrl + w)s - split windows
* (ctrl + w)w - switch between windows
* (ctrl + w)q - quit a window
* (ctrl + w)v - split windows vertically

# Plugin
`\t` - open NERDTree tab

`\b` - open definition list

ctrl + n - select next occurrence
