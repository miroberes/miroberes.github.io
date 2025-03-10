---
title: Vim for CKAD - exercise 4 - hide, show vim
date: 2025-01-11
---
Previous: [Vim for CKAD - exercise 3 - start writing](https://miroberes.github.io/CKAD-Exam-Tips-vim-exercises/CKAD-Exam-Tips-vim-exercises-003-start-writing.html)

Vim is great if we invest time to learn to use it. Editing Kubernetes YAML files in Vim should be as natural as breathing.

### Getting Started with Vim for CKAD exam

#### First things first
Put your mouse away, forget you have a touchpad and **keep your hands on the keyboard**.

#### Open a new file in Vim
```
vim vimisawesome.yaml
```
If the file vimisawesome.yaml does not exist, Vim opens a new file.

#### Send vim to background
If you viewed some resource in the terminal and need to see the output again, but now you are in vim editing a document, don't leave Vim, just hide it.

Put vim to sleep, ***z***zzz...

```
control z
```

The output in the terminal shows that Vim is stopped:
```
[1]+  Stopped                 vim vimisawesome.yaml
```
#### Bring vim to ***f***ore***g***round

```
fg
```

Now you can continue to edit the file in Vim.

#### Congratulations!
Practice these steps to make using Vim second nature, hiding it and bringing it back instead of closing it and opening again.

Next: [Vim for CKAD - exercise 5 - hide vim, edit multiple files](https://miroberes.github.io/CKAD-Exam-Tips-vim-exercises/CKAD-Exam-Tips-vim-exercises-005-edit-multiple-files.html)
