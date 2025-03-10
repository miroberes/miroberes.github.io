---
title: kubectl for CKAD - exercise 5 - run a multi-container pod
date: 2025-01-12
---
Previous: [kubectl for CKAD - exercise 4 - set resources and replace pod](https://miroberes.github.io/CKAD-Exam-Tips-kubectl-exercises/CKAD-Exam-Tips-kubectl-exercises-004-set-resources-replace-pod.html)

#### First things first
Put your mouse away, forget you have a touchpad and **keep your hands on the keyboard**.

#### Start your terminal, the adventure continues...

The situation with the CKAD and other CK* exams is that we need to be ***fast***. To be fast when creating YAML manifests for multi-container pods, you can generate them on the fly with `kubectl` and `sed`, then run `kubectl apply` right from inside Vim!

### Scenario
A pod named `multipod` needs to be created in the cluster with two containers:  
- `multipod` using the image `nginx:alpine` - name assigned automatically
- `busybox-container` using the image `busybox:latest`  

We will generate the pod manifest on the fly and edit it in Vim for quick application. Since `kubectl run` allows only one --image flag to be used, we will combine it with`sed`.
#### Use `kubectl run` and `sed` to create a multi-container pod manifest
```
kubectl run multipod --image nginx:alpine --dry-run=client -o yaml \
 | sed "/containers:/a\  - image: busybox:latest\n    name: busybox-container\n    command: ['sh','-c','sleep 3600']" | vim -
```

1. `kubectl run` generates the basic YAML manifest for a pod with one container (`nginx:alpine`), the pipe `|` passes the YAML to `sed`
2. `sed` appends the definition for a second container (`busybox-container`) directly after the `containers:` key. Count spaces for correct indentation!
3. `vim -` opens the output for editing and further customization

#### Check if the indentation is correct and save the YAML manifest
```
:w multipod.yaml
```

#### Run `kubectl apply` directly from inside Vim
```
:!kubectl apply -f %   # % references the current file name
```

If everything is fine with the yaml file, multipod.yaml gets applied, pod multipod is created.

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
If the error is fixed in the yaml file, multipod.yaml gets applied, pod multipod is created.

#### Close the document / ***q***uit Vim
```
:q
```
Vim ***q***uits, closing the edited pod manifest.

#### Congratulations!  
You've learned to generate a multi-container pod on the fly using `kubectl`, `sed`, and `Vim`. Practice often to master fast YAML generation and direct editing for the CKAD exam.

Next: [kubectl for CKAD - exercise 6 - set env to a multi-container pod](https://miroberes.github.io/CKAD-Exam-Tips-kubectl-exercises/CKAD-Exam-Tips-kubectl-exercises-006-set-env-multi-pod.html)
