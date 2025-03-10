---
title: Vim for CKAD - exercise 2 - append text, save as
date: 2025-01-11
---
Previous: [Vim for CKAD - exercise 1 - creating, editing, saving, and closing documents in Vim](https://miroberes.github.io/CKAD-Exam-Tips/CKAD-Exam-Tips-vim-exercises/CKAD-Exam-Tips-vim-exercises-001.html)

Vim is great if we invest time to learn to use it. Editing Kubernetes YAML files in Vim should be as natural as breathing.

### Getting Started with Vim for CKAD exam

#### First things first
Put your mouse away, forget you have a touchpad and **keep your hands on the keyboard**.

#### Open an existing file in Vim
```
vim vimisawesome.yaml
```

#### The text reads something like this
```
Vim is awesome.
```

#### Keep the existing text and ***A***ppend some more text to the end aka start writing at the end of this line
```
A
```
***A*** moves cursor to the end of this line and switches to INSERT mode allowing us to ***A***ppend text.

#### Start writing at the current cursor position
```
 Because it really is!
```
#### ESCape from INSERT mode and ***w***rite (save) the document
```
ESC
:w
```

#### Alternatively, use ***saveas*** to save the document under a new name
```
:saveas newfilename.yaml
```
Vim saves the current document as a new file and opens the copy for editing, closing the original document.

#### Close the document / ***q***uit Vim
```
:q
```
Vim ***q***uits, closing the edited document.

#### Congratulations!
Practice these steps to make creating, editing, saving, and closing YAML files in Vim second nature.

Next: [Vim for CKAD - exercise 3 - start writing](https://miroberes.github.io/CKAD-Exam-Tips/CKAD-Exam-Tips-vim-exercises/CKAD-Exam-Tips-vim-exercises-003-start-writing.html)
