---
title: kubectl for CKAD - exercise 6 - set env to a multi-container pod
date: 2025-01-12
---
Previous: [kubectl for CKAD - exercise 5 - run a multi-container pod](https://miroberes.github.io/CKAD-Exam-Tips-kubectl-exercises/CKAD-Exam-Tips-kubectl-exercises-005-run-multi-container-pod.html)

#### First things first
Put your mouse away, forget you have a touchpad and **keep your hands on the keyboard**.

#### Start your terminal, the adventure continues...

### Scenario
Setting environment variable for a specific container in a multi-container pod.

A pod named `multipod` is running in the cluster with two containers named: `multipod` and `busybox-container`. We want to set the environment variable `APP_MODE=production` on the `busybox-container` container only.

If you don't have the pod `multipod` running in your cluster from the previous exercise: [kubectl for CKAD - exercise 5 - run a multi-container pod](https://miroberes.github.io/CKAD-Exam-Tips-kubectl-exercises/CKAD-Exam-Tips-kubectl-exercises-005-run-multi-container-pod.html)

#### Attempting to directly set the environment variable for a specific container throws an error:
```
kubectl set env pod/multipod APP_MODE=production --containers=busybox-container
```
Output:
```
error: failed to patch env update to pod template: Pod "multipod" is invalid: spec: Forbidden: pod updates may not change fields other than ...
```

But, there is a way around it! 

#### Use `kubectl set env`  with `--dry-run=client` to modify the pod manifest,  targeting the specific container:
```
kubectl set env pod/multipod APP_MODE=production --containers=busybox-container --dry-run=client -o yaml | vim -
```

Vim opens the YAML file. Ensure the `APP_MODE=production` variable is added under the correct container (`busybox-container`).

#### Save the document as a new file (for example, `new-multipod.yaml`)
```
:w new-multipod.yaml
```

#### Replace the running pod with the updated definition directly from Vim
```
:!kubectl replace -f % --force --grace-period 0
```

#### Verify the environment variables for the busybox-container:
```
kubectl set env pods multipod -c busybox-container --list
```
If everything is fine, the command will output::
```
# Pod multipod, container busybox-container
APP_MODE=production
```
#### Alternatively, you can also verify the APP_MODE value with:
```
kubectl exec multipod -c busybox-container -- env | grep APP_MODE
```
Output:
```
APP_MODE=production
```

#### Congratulations!
Practice these steps to master setting container-specific environment variables in multi-container pods with kubectl and Vim.

Next: [kubectl for CKAD - exercise 7 - use bash exported variables in Vim 1](https://miroberes.github.io/CKAD-Exam-Tips-kubectl-exercises/CKAD-Exam-Tips-kubectl-exercises-007-use-bash-variables1.html)
