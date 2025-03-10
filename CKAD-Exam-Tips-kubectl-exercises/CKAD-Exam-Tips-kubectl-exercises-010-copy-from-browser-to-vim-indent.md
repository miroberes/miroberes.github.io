---
title: kubectl for CKAD - exercise 10 - copy from browser, paste into Vim, indent
date: 2025-01-18
---
Previous: [kubectl for CKAD - exercise 9 - expose a pod with NodePort service, delete lines in Vim](https://miroberes.github.io/CKAD-Exam-Tips/CKAD-Exam-Tips-kubectl-exercises/CKAD-Exam-Tips-kubectl-exercises-009-expose-nodeport-delete-lines.html)


#### First things first
Put your mouse away, forget you have a touchpad and **keep your hands on the keyboard**.

For resources that cannot be created with `kubectl`, we need to copy YAML definitions from kubernetes.io. Sometimes we need to copy some lines into a deployment yaml, but there is only a pod yaml having the lines we need. When we paste the lines from a pod yaml into the deployment yaml, the indentation is incorrect. In this exercise, we will see how to fix the indentation.

### Scenario
Create a deployment which uses an `emptyDir: {}` volume named `redis-storage` and mounts it to the mount path `/data/redis`

#### Open a new deployment yaml manifest directly in Vim
```
kubectl create deploy testdep --image nginx:alpine --dry-run=client -o yaml | vim -
```

Use the alias k and the bash variable $dry if you have already created them in your .bashrc file. ([kubectl for CKAD - exercise 8 - use bash exported variables in Vim 2](https://miroberes.github.io/CKAD-Exam-Tips/CKAD-Exam-Tips-kubectl-exercises/CKAD-Exam-Tips-kubectl-exercises-008-use-bash-variables2.html))

#### Output in Vim:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: testdep
  name: testdep
spec:
  replicas: 1
  selector:
    matchLabels:
      app: testdep
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: testdep
    spec:
      containers:
      - image: nginx:alpine
        name: nginx
        resources: {}
status: {}
```


To keep the pasted yaml lines properly formatted, we need to set Vim up:
```
:set paste
```

For proper yaml formatting, the indentation needs to be done with spaces. To see if yaml is not accidentally indented with tabs ( ```^I``` ), make tabs visible :
```
:set list
```

#### Copy lines for `volumes` and `volumeMounts` from the pod yaml on kubernetes.io - paste it into a Vim document

Open the page [https://kubernetes.io/docs/tasks/configure-pod-container/configure-volume-storage/#configure-a-volume-for-a-pod](https://kubernetes.io/docs/tasks/configure-pod-container/configure-volume-storage/#configure-a-volume-for-a-pod)
and have a look at the pods/storage/redis.yaml.

#### This is the part we want to copy and paste into our deployment yaml:
```
    volumeMounts:
    - name: redis-storage
      mountPath: /data/redis
  volumes:
  - name: redis-storage
    emptyDir: {}
```

Practice using the Firefox browser in Linux (i hope, you are using a linux machine all the time).

#### Select the yaml text with the mouse and copy it.
```
control c
```
#### Back in Vim, look up the container name (`name: nginx`), then jump right to it by searching in Vim starting at the bottom
```
G               # jump down to the last line
?ngi            # use ? to search upwards from the bottom to top. Be lazy, there is no need to write the whole word nginx
Enter
n               # 'n'ew search up - repeats the search upwards if needed
```

With the cursor positioned on the `name: nginx`, paste the copied yaml lines into the deployment yaml:
```
o                  # o opens a new line be`low` (as in `low`ercase `o`) the current line and switches to INSERT mode, allowing us to paste
shift control v
```

If we keep the indentation from the pod yaml definition, the deployment will not work.
```
 ...   
    spec:
      containers:
      - image: nginx:alpine
        name: nginx
    volumeMounts:
    - name: redis-storage
      mountPath: /data/redis
  volumes:
  - name: redis-storage
    emptyDir: {}
        resources: {}
status: {}
```

To correct the indentation, we will nudge the copied lines to the right, so that it looks like this:
```
...
    spec:
      containers:
      - image: nginx:alpine
        name: nginx
        volumeMounts:
        - name: redis-storage
          mountPath: /data/redis
      volumes:
      - name: redis-storage
        emptyDir: {}
        resources: {}
status: {}
```

!!! Now reset the **paste** mode back to **nopaste**, otherwise the tab expansion (nudging) will not work. We need Vim to expand tabs into two spaces when we press Tab, instead of inserting the ```^I```character.
```
ESC                  # 'ESC'ape from INSERT mode to NORMAL modes
:set nopaste 
```

Search upwards for the first line to be nudged
```
?vol                 # use ? to search upwards from the bottom to top. Be lazy, there is no need to write the whole word volumeMounts
Enter
n                    # new search up - repeat the search upwards until the cursor lands the first line of the copied block
```

#### Set Vim up for correct indentation
```
:set expandtab       # :set et    works as well
:set tabstop=2       # :set ts=2
:set shiftwidth=2    # :set sw=2
```

With the cursor now positioned on the first line to be nudged, the question is, how many lines do we need to nudge?
#### Set Vim up to show line ***nu***mbers and ***r***elative line ***nu***mbers
```
:set nu           # show 'nu'mbers
:set rnu          # show 'r'elative 'nu'mbers
```

Or do it in one line
```
:set nu rnu
```

For correct yaml formatting, indentation needs to be done with spaces only. Anytime there are some ```^I``` characters visible, something has gone wrong.
#### If not done already - to check if we are not indenting the lines with tabs, make the tabs visible. 
```
:set list    
```

Ignore the `$` character, `:set list` shows us also the end of the line.

#### Notice, that the relative numbering starts below the current line, so we need to add 1 to the number of lines to nudge (5 + 1).
```
  4     spec:$
  3       containers:$
  2       - image: nginx:alpine$
  1         name: nginx$
23      volumeMounts:$
  1     - name: redis-storage$
  2       mountPath: /data/redis$
  3   volumes:$
  4   - name: redis-storage$
  5     emptyDir: {}$
  6         resources: {}$
  7 status: {}$
```

#### Indent 6 lines 
```
6>>              # nudge 6 lines to the right
.                # dot repeats it once more for all 6 lines
```

```
--- if you need to go back, use undo ---
u
```

#### ***w***rite (save) the document
```
ESC                    # 'ESC'ape from INSERT mode 
:w testdep.yaml        # 'w'rite (save) the document
```

### Remember, we don't leave Vim for this:

#### If you have already set up the bash variable / shortcut `ka` in the previous exercises use it like this directly from inside Vim
```
:!$ka -f %             # Run `kubectl apply` directly from inside Vim
```

#### otherwise, run `kubectl apply` 
```
:!kubectl apply -f %   # % references the current file name
```

#### If everything is fine with the yaml definition, testdep.yaml gets applied, deployment is created
```
kubectl get deploy -o wide             # view the deployment, -o wide is optional, it shows more info about resources
```

### Scenario continued
If there is an error and **the deployment has not been created**, read the error, then simply press Enter go back to Vim, edit the yaml file and correct the error.
#### ***w***rite (save) the document
```
ESC                    # 'ESC'ape from INSERT mode 
:w testdep.yaml        # 'w'rite (save) the document
```

#### Repeat running `kubectl apply` directly from Vim re-using the previous command
```
:up arrow              # repeat to find the command you used to apply the yaml
Enter
```

#### If the error is fixed in the yaml file, testdep.yaml gets applied, deployment is created
```
k get deploy -o wide             # view the deployment, -o wide is optional, it shows more info about resources
```

#### Congratulations!
Practice these steps to make editing documents in Vim second nature, copying yaml text from a browser, pasting it into Vim documents and correcting the indentation.

Next: [kubectl for CKAD - exercise 11 - search and replace in Vim](https://miroberes.github.io/CKAD-Exam-Tips/CKAD-Exam-Tips-kubectl-exercises/CKAD-Exam-Tips-kubectl-exercises-011-search-and-replace-in-Vim.html)