---
title: Vim for CKAD - exercise 5 - hide vim, edit multiple files
date: 2025-01-11
---
Previous: [Vim for CKAD - exercise 4 - hide, show vim](https://miroberes.github.io/CKAD-Exam-Tips-vim-exercises/CKAD-Exam-Tips-vim-exercises-004-hide-show.html)

Vim is great if we invest time to learn to use it. Editing Kubernetes YAML files in Vim should be as natural as breathing.

### Getting Started with Vim for CKAD exam

#### First things first
Put your mouse away, forget you have a touchpad and **keep your hands on the keyboard**.

#### Open a new file in Vim
```
vim vimisawesome1.yaml
```

#### Send vim to background - edit multiple files
If you are editing a document in Vim and need to edit another, don't leave Vim, just hide it.

Put the first Vim to sleep, ***z***zzz...

```
control z
```

The output in the terminal shows that Vim is stopped:
```
[1]+  Stopped                 vim vimisawesome.yaml
```

#### Open another file in Vim
```
vim vimisawesome2.yaml
```
#### Put the second Vim to sleep, ***z***zzz...

```
control z
```

#### Show all Vim sleeping ***jobs***

``` bash
jobs
[1]-  Stopped                 vim vimisawesome1.yaml
[2]+  Stopped                 vim vimisawesome2.yaml
```
#### Bring the first Vim to ***f***ore***g***round

```
fg 1
```

Now you can continue to edit the file in Vim.

#### Hide the first Vim ***z***zzz..., bring the second Vim to ***f***ore***g***round

```
control z

fg 2
```

#### Congratulations!
Practice these steps to make editing multiple documents in Vim second nature, hiding it and bringing it back instead of closing it and opening again.

Next: [Vim for CKAD - exercise 6 - copy from browser, paste into Vim](https://miroberes.github.io/CKAD-Exam-Tips-vim-exercises/CKAD-Exam-Tips-vim-exercises-006-copy-paste-in-vim.html)
