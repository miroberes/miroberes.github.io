---
title: Vim for CKAD - exercise 3 - start writing
date: 2025-01-11
---
Previous: [Vim for CKAD - exercise 2 - append text, save as](https://miroberes.github.io/CKAD-Exam-Tips-vim-exercises/CKAD-Exam-Tips-vim-exercises-002-append-save-as.html)

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
Vim is awesome. Because it really is!
```

#### Keep the existing text and ***o***pen a new line below the current line - aka start writing below this line
```
o
```
***o*** ***o***pens a new line l***o***wer (as in l***o***wercase ***o***) than the current one and switches to INSERT mode, allowing us to start writing

#### Start writing at the current cursor position
```
A new line has opened low below the current line.
```

#### ESCape from INSERT mode and return to NORMAL mode before proceeding to the next step.
```
ESC
```

#### Keeping the existing text ***O***pen a new line above the current line - aka start writing above this line

```
O
```

***O*** ***O***pens a new line up (as in uppercase ***O***) ab***O***ve the current one and switches to INSERT mode, allowing us to start writing.

#### Start writing at the current cursor position
```
A new line has opened up abOve the current line.
```

#### ESCape from INSERT mode and return to NORMAL mode before proceeding to the next step.
```
ESC
```
#### Now that the exercise pattern is clear, practice these most commonly used INSERT mode commands

#### Start writing – remember to ESCape from INSERT mode after each write.

r***i***ght where you are
```
i
```

before the first non-blank character on this line (***I***nitial)
```
I
```

move to position ***0*** and start writing r***i***ght there aka before the first character on this line
```
0i
```

***A***ppend after the last character on this line
```
A
```

***a***fter this character aka one character to the right
```
a
```

on a new line up ab***O***ve the current line
```
O
```

on a new line l***o***w below this line
```
o
```

from the ***S***tart of this line but delete all the text
```
S
```

right where you are but delete the ***s***elected character
```
s
```

 ***C***lear (delete) everything from the cursor to the end of this line and start writing (enter INSERT mode)
```
C
```

#### Combining INSERT mode commands with movements up ***k*** and down ***j***

***A***ppend to the end of the existing line above
```
kA
```

at the beginning of the existing line above (***I***nitial)
```
kI
```

***A***ppend to the end of the existing line below
```
jA
```
at the beginning of the existing line below (***I***nitial)
```
jI
```

#### ESCape from INSERT mode 
```
ESC
```

#### ***w***rite (save) the document if you want to
```
:w
```

#### Alternatively, if you don't want to write the document, close it without saving aka - ***q***uit forcefully (!)
```
:q!
```
Vim ***q***uits, closing the edited document, discarding all changes.

#### Congratulations!
Practice these steps to make editing YAML files in Vim second nature.

Next: [Vim for CKAD - exercise 4 - hide, show vim](https://miroberes.github.io/CKAD-Exam-Tips-vim-exercises/CKAD-Exam-Tips-vim-exercises-004-hide-show.html)
