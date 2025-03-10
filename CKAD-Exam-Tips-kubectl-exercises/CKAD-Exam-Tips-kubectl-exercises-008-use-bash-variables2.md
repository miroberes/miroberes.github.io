---
title: kubectl for CKAD - exercise 8 - use exported bash variables in Vim 2
date: 2025-01-13
---
Previous: [kubectl for CKAD - exercise 7 - use bash exported variables in Vim 1](https://miroberes.github.io/CKAD-Exam-Tips/CKAD-Exam-Tips-kubectl-exercises/CKAD-Exam-Tips-kubectl-exercises-007-use-bash-variables1.html)
#### First things first
Put your mouse away, forget you have a touchpad and **keep your hands on the keyboard**.

### Scenario
When creating kubernetes resources, the `kubectl` commands can get quite lengthy. To be ***fast and efficient***, especially when working with the `--dry-run -o yaml` flag frequently, use bash aliases and variables.
#### The original command we don't have to write in its full length
```
kubectl run longpod --image=nginx:alpine --dry-run=client -o yaml --command -- bin/sh -c "echo test;sleep 100" | vim -
```
#### Set up bash alias `k` and exported variable `dry`
```
vim ~/.bashrc                          # open the .bashrc file
G                                      # 'G'o to the last line
i                                      # switch to the INSERT mode
alias k=kubectl                        # add this line if it is not already somewhere in the .bashrc file
export dry='--dry-run=client -o yaml'  # add this line
```
#### ***w***rite (save) the .bashrc document
```
ESC              # 'ESC'ape from INSERT mode 
:w               # 'w'rite (save) the document
:q               # 'q'uit Vim
```
#### ***source*** (reload) your .bashrc
```
source ~/.bashrc
```
#### Open a new yaml file directly in Vim using the alias `k` (without `$`) and the exported bash variable `dry` (with `$`)
```
k run shortpod --image=nginx:alpine $dry --command -- bin/sh -c "echo test;sleep 100" | vim -
```
#### Congratulations!
You've learned how to use command expansion with bash variables to make your kubectl commands more efficient. This will save you valuable time during the CKAD exam.

Next: [kubectl for CKAD - exercise 9 - expose a pod with NodePort service, delete lines in Vim](https://miroberes.github.io/CKAD-Exam-Tips/CKAD-Exam-Tips-kubectl-exercises/CKAD-Exam-Tips-kubectl-exercises-009-expose-nodeport-delete-lines.html)