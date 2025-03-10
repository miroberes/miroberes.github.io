---
title: Vim for CKAD - exercise 6 - copy from browser, paste into Vim
date: 2025-01-11
---
Previous: [Vim for CKAD - exercise 5 - hide vim, edit multiple files](https://miroberes.github.io/CKAD-Exam-Tips/CKAD-Exam-Tips-vim-exercises/CKAD-Exam-Tips-vim-exercises-005-edit-multiple-files.html)

Vim is great if we invest time to learn to use it. Editing Kubernetes YAML files in Vim should be as natural as breathing.

### Getting Started with Vim for CKAD exam

#### First things first
Put your mouse away, forget you have a touchpad and **keep your hands on the keyboard**.

#### Open a new file in Vim
```
vim vimisawesome.yaml
```

To keep the yaml properly formatted after pasting it into Vim, we need to set Vim up:
```
:set paste
```

For correct yaml formatting, the indentation needs to be done with spaces. To see if yaml is not accidentally indented using tabs (```^I``` ), make tabs visible :
```
:set list
```
#### Copy a yaml from kubernetes.io - paste it into a Vim document

Open the networkpolicy.yaml file from:
[https://kubernetes.io/docs/concepts/services-networking/network-policies/#networkpolicy-resource](https://kubernetes.io/docs/concepts/services-networking/network-policies/#networkpolicy-resource)

Practice using the Firefox browser in Linux: select the yaml text with the mouse and copy it.
```
control c
```

Paste the yaml into the Vim document:
```
i  // switch to INSERT mode
shift control v
```

If the yaml looks good - meaning the indentation is done with spaces (NOT with multiple ```^I``` characters, which would now be visible)
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

The last but very important step is to reset the **paste** mode back to **nopaste**, otherwise the tab expansion will not work. We need Vim to expand tabs into two spaces when we press Tab, instead of inserting the ```^I```character.
```
:set nopaste 
```

#### Congratulations!
Practice these steps to make editing documents in Vim second nature, copying yaml text from a browser and pasting it into Vim documents without loosing the proper indentation.
