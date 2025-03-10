---
title: Bash for CKAD - exercise 1 - bash substitution
date: 2025-01-12
---

### Getting Started with Bash for CKAD exam

#### First things first
Put your mouse away, forget you have a touchpad and **keep your hands on the keyboard**.

#### Be fast when fixing typos in Bash commands
Mistakes happen, but in the CKAD exam, correcting them quickly is key. Instead of retyping the entire command or awkwardly navigating with arrow keys, use Bash quick substitution syntax to fix typos.

#### A possible CKAD exam scenario 1:

Correct a typo in a Kubernetes command

#### Get pod with this command:
```
kubectl get p --namespace default -o wide --show-labels
```

Error:
```
error: the server doesn't have a resource type "p"
```

Since there is only one typo to be corrected we can use the bash quick substitution syntax: `^oldstring^newstring`. To tell the substitution which `p` it should replace with `po` we need to add the `get` word to the string.

#### Re-run the previous command but replace `get p` with `get po`:
```
^get p^get po
```

Output:
```
kubectl get po --namespace default -o wide --show-labels
```

### Scenario 2:

Run two pods, redpod with APP_COLOR=red and greenpod with APP_COLOR=green
```
kubectl run redpod --image nginx:alpine --env APP_COLOR=red
```
Output:
```
pod/redpod created
```

Since now there are multiple words to be replaced in the previous command, we can not use the `^old^new` syntax, because it only replaces a single word.
But we still can reuse the previous command with bash global substitution (gs).

#### Re-run the previous command but replace every instance of `red` with `green`:
```
!!:gs/red/green
```

Output:
```
kubectl run greenpod --image nginx:alpine --env APP_COLOR=green
pod/greenpod created
```
#### Congratulations!
Master these Bash substitution techniques to correct typos and reuse commands instantly without breaking your flow during the CKAD exam.
