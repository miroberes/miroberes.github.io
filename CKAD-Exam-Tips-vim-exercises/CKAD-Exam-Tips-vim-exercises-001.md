---
title: Vim for CKAD - exercise 1 - creating, editing, saving, and closing documents in Vim
date: 2025-01-11
---
Vim is great if we invest time to learn to use it. Editing Kubernetes YAML files in Vim should be as natural as breathing.

### Getting Started with Vim for CKAD exam

#### First things first
Put your mouse away, forget you have a touchpad and **keep your hands on the keyboard**.

####  Start your terminal, the adventure begins... 

#### Open a new file in Vim
```
vim vimisawesome.yaml
```
If the file vimisawesome.yaml does not exist, Vim opens a new file.

#### Switch to INSERT mode to start writing at the current cursor position
```
i
```
**i** stands for INSERT mode, which is used for editing, as opposed to NORMAL mode, which is used for navigation, commands, and text manipulation.

When we first open Vim, we'll be in NORMAL mode; that is why we need to switch to INSERT mode.

#### Start writing at the current cursor position
```
Vim is awesome.
```
... because it really is! :)

#### ESCape from INSERT mode and ***w***rite (save) the document
```
ESC
:w
```
ESC switches back to NORMAL mode. Vim remembers the filename we started it with, so it ***w***rites a file vimisawesome.yaml to the disk.

#### Close the document / ***q***uit Vim
```
:q
```
Vim ***q***uits, closing the edited document.

#### Congratulations!
Practice these steps to make creating, editing, saving, and closing YAML files in Vim second nature.

Next: [Vim for CKAD - exercise 2 - append text, save as](https://miroberes.github.io/CKAD-Exam-Tips-vim-exercises/CKAD-Exam-Tips-vim-exercises-002-append-save-as.html)
