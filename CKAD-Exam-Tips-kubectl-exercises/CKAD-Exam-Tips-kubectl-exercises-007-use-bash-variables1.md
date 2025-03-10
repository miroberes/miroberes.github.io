---
title: kubectl for CKAD - exercise 7 - use exported bash variables in Vim 1
date: 2025-01-12
---
Previous: [kubectl for CKAD - exercise 6 - set env to a multi-container pod](https://miroberes.github.io/CKAD-Exam-Tips-kubectl-exercises/CKAD-Exam-Tips-kubectl-exercises-006-set-env-multi-pod.html)

#### First things first
Put your mouse away, forget you have a touchpad and **keep your hands on the keyboard**.

### Scenario
We can use the `kubectl` command in Vim directly. But to be fast in Vim, we have no time to write `kubectl` every time. We need to use it as an exported bash variable `k`.
#### Set up a bash exported variable
In your `.bashrc` file, add the following line to export the variable `k` for `kubectl` - notice, that we are **not** creating an alias here.
If the alias k is already there, don't worry it can stay there as well.
```
vim ~/.bashrc       # open the .bashrc file
G                   # 'G'o to the last line
i                   # switch to the INSERT mode
export k=kubectl    # add this line for the bash variable k
```
#### ***w***rite (save) the .bashrc document
```
ESC              # 'ESC'ape from INSERT mode 
:w               # 'w'rite (save) the document
:q               # 'q'uit Vim
```

After adding the line, reload your `.bashrc`:
```
source ~/.bashrc
```

Open a new pod yaml file in Vim
```
kubectl run testpod --image nginx:alpine --dry-run=client -o yaml | vim -
```
#### ***w***rite (save) the document
```
ESC              # 'ESC'ape from INSERT mode 
:w testpod.yaml  # 'w'rite (save) the document
```

#### Use the exported bash variable `k` in Vim
To apply the YAML file using the `kubectl` command via the exported variable `k`, we need to prefix it with `$` like this:
```
:!$k apply -f %   # % references the current file name
```
This command will execute `kubectl apply -f` on the current file being edited in Vim.

### You are probably starting to see, where this is going to go!

We can shorten the `kubectl apply` the same way to `ka` and use it like this
```
:!$ka -f %
```

The  command `kubectl replace` can be shortened to `kr` and `--force --grace-period 0` can be shortened to `f0`.
```
:!kubectl replace -f % --force --grace-period 0
```
becomes
```
:!$kr -f % $f0
```
#### If it looks strange to you now, don't worry. Just start using it, and you will never want to go back to fully written kubectl commands again.

### Let's create those shortcuts

#### Close the document / ***q***uit Vim, otherwise it will not know about the changes in .bashrc
```
:q
```

#### Set up bash exported variables
```
vim ~/.bashrc                          # open the .bashrc file
G                                      # 'G'o to the last line
i                                      # switch to the INSERT mode
export ka='kubectl apply'              # add this line for the bash variable ka
export kr='kubectl replace'            # add this line for the bash variable kr
export f0='--force --grace-period 0'   # add this line for the bash variable f0 - or use any other name for this shortcut
```
#### ***w***rite (save) the .bashrc document
```
ESC              # 'ESC'ape from INSERT mode 
:w               # 'w'rite (save) the document
:q               # 'q'uit Vim
```

After adding the shortcuts, reload your `.bashrc`:
```
source ~/.bashrc
```

#### Open a new pod yaml file in Vim
```
kubectl run testpod2 --image nginx:alpine --dry-run=client -o yaml | vim -
```
#### ***w***rite (save) the document
```
ESC              # 'ESC'ape from INSERT mode 
:w testpod2.yaml  # 'w'rite (save) the document
```

#### Use the exported bash variable `ka`a in Vim
To apply the YAML file using the `kubectl` command via the exported variable `k`, we need to prefix it with `$` like this:
```
:!$ka -f %   # % references the current file name
```
This command will execute `kubectl apply -f` on the current file being edited in Vim.

#### Use the exported variables `kr` and `f0` in Vim
```
:!$kr -f % $f0
```
The running pod is deleted with `--force --grace-period 0` and a new pod is created.

#### Congratulations!
Practice these steps to make applying and replacing Kubernetes resources as fast as possible, using bash exported variables directly in Vim.

Next: [kubectl for CKAD - exercise 8 - use bash exported variables in Vim 2](https://miroberes.github.io/CKAD-Exam-Tips-kubectl-exercises/CKAD-Exam-Tips-kubectl-exercises-008-use-bash-variables2.html)
