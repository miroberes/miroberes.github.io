---
title: kubectl for CKAD - exercise 11 - search and replace in Vim
date: 2025-01-19
---
Previous: [kubectl for CKAD - exercise 10 - copy from browser, paste into Vim, indent](https://miroberes.github.io/CKAD-Exam-Tips-kubectl-exercises/CKAD-Exam-Tips-kubectl-exercises-010-copy-from-browser-to-vim-indent.html)

In this exercise, we will learn how to search and replace text within a Vim document. We will be using the final `testdep.yaml` file created in the previous exercise.

### Scenario 1: Basic Search and Replace

Change the `mountPath:` from `/data/redis` to `/opt/redis`

#### Open the `testdep.yaml` file from the previous exercise in Vim

```
vim testdep.yaml
```

The yaml should look like this:

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
        volumeMounts:
        - name: redis-storage
          mountPath: /data/redis
      volumes:
      - name: redis-storage
        emptyDir: {}
        resources: {}
status: {}
```

### Basic search and replace

#### Search and replace pattern:
```
:%s/searchtext/replacementtext/gc
```

Let's break this down:

-   `%s/`     perform a substitution, the `%` means to perform the substitution across all lines in the file
-   `searchtext`     text to search for
-  `/`     delimiter
-   `replacementtext`     replacement text
-   `/`     delimiter
-   `g`     ***g***lobal, replace all occurrences of `searchtext` everywhere in the document
-   `c`     ***c***onfirm, ask for confirmation before replacing the text. You can omit it if you want to replace all found `searchtext` occurrences

To search for `data` and replace it with `opt`, use the following command in Vim (in NORMAL mode, after pressing `ESC`):

```
:%s/data/opt/gc
```


Vim will now highlight the first occurrence of 'data', and at the bottom of the screen ask:

```
replace with opt (y/n/a/q/l/^E/^Y)?
```

The most used operations:
- Press `y` to replace the current occurrence.
- Press `n` to skip the current occurrence and move to the next.

Rest of the operations:
- Press `a` to replace all occurrences without asking for confirmation.
- Press `q` to quit the replace process.
- Press `l` to replace the current occurrence and quit the replace process.
- Press `^E` or `^Y` to scroll the screen up or down, respectively.

Use whichever operation you need, so the `mountPath` in your file changes to `/opt/redis`.

#### Write (save) the document
```
ESC                    # 'ESC'ape from INSERT mode 
:w testdep.yaml        # 'w'rite (save) the document
```

### Apply the changes, to check if the yaml is correct - directly from inside Vim

If you have already set up the bash variable / shortcut `ka` in the previous exercises use it like this
```
:!$ka -f %             # Run `kubectl apply` directly from inside Vim, % references the current file name
```

Otherwise, you can run `kubectl apply`
```
:!kubectl apply -f %
```

### Scenario 2: Escaping special characters

Sometimes the text we want to search for contains special characters like `/`. We can escape these characters with a backslash `\` in the search pattern.

Let's change `/opt/redis` back to `/data/redis`.

In Vim, execute:
```
:%s/\/opt\/redis/\/data\/redis/gc
```
Here we are using `\/` to escape `/` in the searched text, using the slashes after `:%s` and `redis` as delimiters (`:%s/` ...`redis/`...`redis/gc` ) 
Again, with `gc` Vim will ask for confirmation before replacing text.

#### Write (save) the document
```
ESC                    # 'ESC'ape from INSERT mode 
:w testdep.yaml        # 'w'rite (save) the document
```

### Replace the running deployment, to check if the yaml is correct - directly from inside Vim  {#replace-deployment}

If you have already set up the bash variables / shortcuts `kr` and `$f0` in the previous exercises use it like this
```
:!$kr -f % $f0            # Run `kubectl replace` from inside Vim
```

Otherwise, you can run `kubectl replace`
```
:!kubectl replace -f % --force --grace-period 0
```

### Scenario 3: Using an alternative delimiter

When we need to replace text containing  forward slashes, instead of escaping `/` with `\/`, we can change the delimiter in our search and replace pattern. Common choices include `%`, `#` or `+`.

Let's replace `/data/redis` with `/mnt/redis` using `%` as a delimiter.

```
:%s%/data/redis%/mnt/redis%gc
```

Here, we are using `%` instead of `/` as the delimiter for the search and replace pattern, which makes it easier to work with paths or other text containing slashes.

Again, Vim will ask for confirmation before replacing text.
#### Write (save) the document
```
ESC                    # 'ESC'ape from INSERT mode 
:w testdep.yaml        # 'w'rite (save) the document
```

[Replace the running deployment, to check if the yaml is correct - directly from inside Vim](#replace-deployment)


### Scenario 4: Search and replace only part of a line

Sometimes, you only want to replace part of a line, for example, only the value after a key.
Let's say we want to change the name of the volume mount from `redis-storage` to `redis-volume`.

In your `testdep.yaml`, you have:
```
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

#### We only want to change `storage`, not the whole `redis-storage`
```
:%s/name: redis-\zs.*/volume/gc
```

Let's analyze the command:
-   `%s/`: substitute in all lines
-   `name: redis-`: search for text `name: redis-`
-   `\zs`: This is a special sequence that marks the start of the match. Everything before \zs is used to locate the line, but not included in the replacement.
-   `.*`: Match all the rest of the characters on the line
-   `/volume`: replace the matched text after `name: redis-` with `volume`.
-   `gc`: global, and ask for confirmation

Vim will highlight the first occurrence of text after `name: redis-`, and ask for confirmation. Press `a` to replace all occurrences without asking for confirmation.

Now your file should contain:
```
    spec:
      containers:
      - image: nginx:alpine
        name: nginx
        volumeMounts:
        - name: redis-volume
          mountPath: /data/redis
      volumes:
      - name: redis-volume
        emptyDir: {}
        resources: {}
status: {}
```
#### Write (save) the document
```
ESC                    # 'ESC'ape from INSERT mode 
:w testdep.yaml        # 'w'rite (save) the document
```

[Replace the running deployment, to check if the yaml is correct - directly from inside Vim](#replace-deployment)

### Congratulations!
You've now learned how to perform basic and more advanced search and replace operations in Vim. Practice these techniques to become more efficient at editing YAML manifests and other text files.
