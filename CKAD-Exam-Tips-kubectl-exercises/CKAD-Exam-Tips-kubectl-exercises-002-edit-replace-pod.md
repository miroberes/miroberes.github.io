---
title: kubectl for CKAD - exercise 2 - edit and replace a pod
date: 2025-01-11
---
Previous: [kubectl for CKAD - exercise 1 - run a pod](https://miroberes.github.io/CKAD-Exam-Tips/CKAD-Exam-Tips-kubectl-exercises/CKAD-Exam-Tips-kubectl-exercises-001-run-a-pod.html)
### Getting Started with kubectl for CKAD exam

#### First things first
Put your mouse away, forget you have a touchpad and **keep your hands on the keyboard**.

####  Start your terminal, the adventure continues... 

The situation with the CKAD and other CK* exams is that we need to be ***fast***.
The key to be fast when editing yaml files and applying them with kubectl apply is - ***don't leave Vim***.
Yes, we can run `kubectl apply`, even `kubectl replace` right from inside Vim!

### Scenario
A pod testpod is running in the cluster. We want to change some value in the pod, for example the pod name 'testpod' to 'nginx'.
Most fields in a pod manifest can't be changed for a running pod, but we still can use the kubectl edit command and then replace the pod.
#### Start editing pod testpod in Vim
```
kubectl edit po testpod
```

#### Look up the name value (testpod), then jump right to it by searching in Vim from the top
```
/testp     # search downwards from the top, no need to write the whole word
Enter
n          # repeat search downwards as needed
```

#### Replace the name value - type it or copy & paste it from the task description
```
C          # 'C'lear (delete) everything from the cursor to the end of this line and start writing
nginx
```

#### ***w***rite (save) the document to persist changes
```
ESC         # 'ESC'ape from INSERT mode 
:w          # 'w'rite (save) the document, file name is provided by kubectl edit
```

If we would quit Vim now, the pod would not get updated. We need to replace the edited pod with a new one - and we need to do it as fast as possible.
### Watch the magic... again
#### run `kubectl replace` directly from inside Vim - delete the pod and create a new one
```
:!kubectl replace -f % --force --grace-period 0     # % references the current file name
```

#### If everything is fine with the yaml file, pod is deleted and a new pod is created
```
k get pods -o wide   # view pods, -o wide is optional, it shows more info about resources
```

### Scenario continued
If there is an error and **the pod has not been replaced**, read the error, then simply press Enter to edit the yaml file and correct the error.

#### ***w***rite (save) the document
```
ESC         # 'ESC'ape from INSERT mode 
:w          # 'w'rite (save) the document
```

#### Repeat replacing the pod directly from Vim using the previous command
```
:up arrow    # repeat to find the command :!kubectl replace -f % --force --grace-period 0
Enter
```

#### If everything is fine with the yaml file, pod is replaced
```
k get pods -o wide   # view pods, -o wide is optional, it shows more info about resources
```

#### Close the document / ***q***uit Vim
```
:q
```
Vim ***q***uits, closing the edited pod manifest.

#### Congratulations!
Practice these steps to make replacing running pods with kubectl directly from Vim second nature.

Next: [kubectl for CKAD - exercise 3 - set env and replace pod](https://miroberes.github.io/CKAD-Exam-Tips/CKAD-Exam-Tips-kubectl-exercises/CKAD-Exam-Tips-kubectl-exercises-003-set-env-replace-pod.html)
