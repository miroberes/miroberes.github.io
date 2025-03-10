---
title: kubectl for CKAD - exercise 1 - run a pod
date: 2025-01-11
---
### Getting Started with kubectl for CKAD exam

#### First things first
Put your mouse away, forget you have a touchpad and **keep your hands on the keyboard**.

#### Start your terminal, the adventure begins... 

The situation with the CKAD and other CK* exams is that we need to be ***fast***.
The key to be fast when editing yaml files and applying them with kubectl apply is - ***don't leave Vim***.
Yes, we can run `kubectl apply`, even `kubectl replace` right from inside Vim!

### A possible CKAD exam scenario
Run a pod testpod, name the container nginx-container

#### Open a free kubernetes cluster playground or use your own
[https://killercoda.com/playgrounds/scenario/kubernetes](https://killercoda.com/playgrounds/scenario/kubernetes)

#### Open a new yaml file directly in Vim
```
kubectl run testpod --image nginx:alpine --dry-run=client -o yaml | vim -
```

#### Look up the container name (testpod), then jump right to it by searching in Vim starting at the bottom
```
G         // jump down to the last line
?testpod  // search upwards from the bottom to top
Enter
n         // 'n'ew search - repeat search upwards as needed
```

#### Replace the container name with nginx-container - type it or copy & paste it from the task description
```
C          // 'C'lear (delete) everything from the cursor to the end of this line and start writing
nginx-container
```

#### ***w***rite (save) the document
```
ESC              // 'ESC'ape from INSERT mode 
:w testpod.yaml  // 'w'rite (save) the document
```

### Watch the magic...
#### run `kubectl apply` directly from inside Vim
```
:!kubectl apply -f %   # % references the current file name
```

#### If everything is fine with the yaml file, testpod.yaml gets applied, pod is created.
```
k get pods -o wide   # view pods, -o wide is optional, it shows more info about resources
```

### Scenario continued
If there is an error and **the pod has not been created**, read the error, then simply press Enter to edit the yaml file and correct the error.

#### ***w***rite (save) the document
```
ESC         // 'ESC'ape from INSERT mode 
:w          // 'w'rite (save) the document
```

#### Repeat running `kubectl apply` directly from Vim re-using the previous command
```
:up arrow    // repeat to find the command :!kubectl apply -f %
Enter
```
If the error is fixed in the yaml file, testpod.yaml gets applied, pod is created.

#### ***w***rite (save) the document to persist changes
```
ESC         // 'ESC'ape from INSERT mode 
:w          // 'w'rite (save) the document, file name is provided by kubectl edit
```

#### If everything is fine with the yaml file, testpod.yaml gets applied, pod is created.
```
k get pods -o wide   # view pods, -o wide is optional, it shows more info about resources
```

#### Close the document / ***q***uit Vim
```
:q
```
Vim ***q***uits, closing the edited pod manifest.

#### Congratulations!
Practice these steps to make applying yaml files and running pods with kubectl directly from Vim second nature.

Next: [kubectl for CKAD - exercise 2 - edit and replace a pod](https://miroberes.github.io/CKAD-Exam-Tips-kubectl-exercises/CKAD-Exam-Tips-kubectl-exercises-002-edit-replace-pod.html)
