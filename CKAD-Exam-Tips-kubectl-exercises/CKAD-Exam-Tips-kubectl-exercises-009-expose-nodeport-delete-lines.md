---
title: kubectl for CKAD - exercise 9 - expose a pod with NodePort service, delete lines in Vim
date: 2025-01-14
---
Previous: [kubectl for CKAD - exercise 8 - use bash exported variables in Vim 2](https://miroberes.github.io/CKAD-Exam-Tips-kubectl-exercises/CKAD-Exam-Tips-kubectl-exercises-008-use-bash-variables2.html)
#### First things first
Put your mouse away, forget you have a touchpad and **keep your hands on the keyboard**.

In this exercise we will learn how to create a NodePort service with `kubectl expose`.

Also - we will learn to delete unwanted lines in a yaml file, which is a very important skill for the CKAD and other CK* exams.
For resources which can not be created with `kubectl` we must copy yaml definitions from kubernetes.io. We will have to delete unwanted lines in Vim - and, you guessed it - we need to be **fast and effective** deleting them.

### Scenario
Expose a running pod `nginxpod` on a node through a NodePort service named `nginx-service` on port `30100`. Let’s look at how using different `--dry-run` modes affects the generated YAML.

#### Run a pod
Run an `nginxpod` using the `kubectl` command with port `80` specified.

```
kubectl run nginxpod --image=nginx:alpine --port=80
```

#### Create a NodePort service for the nginxpod,using `kubectl expose` with `--dry-run=client`
```
k expose pod nginxpod --name=nginx-service --type=NodePort --port=80 --dry-run=client -o yaml | vim -
```

#### As you can see, the`nodePort` field in this yaml definition is missing
```
...
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
...
```

#### To include the `nodePort` field, let `kubectl` create it for you with `--dry-run=server`
```
k expose pod nginxpod --name=nginx-service --type=NodePort --port=80 --dry-run=server -o yaml | vim -
```

#### Observe that `--dry-run=server` creates the `nodePort` field and populates it with a random port number:
```
...
spec:
  ports:
  - nodePort: 34567
    port: 80
    protocol: TCP
    targetPort: 80
...
```

#### Replace the autoassigned port with the specified one: `30100`.
```
/34567                    # start searching from top, replace 34567 with the generated number you see in your nodePort: 
Enter
C                         # 'C'lear (delete) everything from the cursor to the end of this line and start writing
30100
ESC                       # 'ESC'ape from INSERT mode
:w nginx-service.yaml     # Save the YAML definition
```

There is one more little problem, let us apply the yaml to see it.
#### Apply the yaml - without leaving Vim!
```
:!kubectl apply -f %      # Run `kubectl apply` directly from inside Vim
```
#### If you have already set up the bash variable / shortcut `ka` in the previous exercise, use it like this:
```
:!$ka -f %  
```

#### It throws an error:
```
The Service "nginx-service" is invalid: spec.clusterIPs: Invalid value: ...

shell returned 1

Press ENTER or type command to continue
```

The error tells us that by using `--dry-run=server -o yaml` we created some unwanted lines - the perfect opportunity to learn deleting lines in Vim.

#### For fast navigation, set Vim up to show line ***nu***mbers and ***r***elative line ***nu***mbers
```
:set nu           # show 'nu'mbers
:set rnu          # show 'r'elative 'nu'mbers
```

Or do it in one line
```
:set nu rnu
```

Line `1` is the one with the cursor so it shows the real line number, all others are relative line numbers, which we will use to make our navigation in Vim **fast and effective**!

```yaml
1   apiVersion: v1
  1 kind: Service
  2 metadata:
  3   creationTimestamp: "2025-01-14T18:27:29Z"
  4   labels:
  5     run: nginxpod
  6   name: nginx-service
  7   namespace: default
  8   uid: 0d175654-b5d4-4306-85c3-924c6f64e62f
  9 spec:
 10   clusterIP: 10.96.0.0
 11   clusterIPs:
 12   - 10.96.0.0
 13   externalTrafficPolicy: Cluster
 14   internalTrafficPolicy: Cluster
 15   ipFamilies:
 16   - IPv4
 17   ipFamilyPolicy: SingleStack
 18   ports:
 19   - nodePort: 32769
 20     port: 80
 21     protocol: TCP
 22     targetPort: 80
...
```

We need to delete lines from`10   clusterIP: 10.96.0.0` down to (including) `17   ipFamilyPolicy: SingleStack`

#### 10 lines ***j***umpdown, to the first line we want to delete
```
10j              # 10 lines 'j'umpdown
```

We landed on the line with the real **nu**mber 11, the first line we want to delete:
```yaml
 10 apiVersion: v1
  9 kind: Service
  8 metadata:
  7   creationTimestamp: "2025-01-14T18:27:29Z"
  6   labels:
  5     run: nginxpod
  4   name: nginx-service
  3   namespace: default
  2   uid: 0d175654-b5d4-4306-85c3-924c6f64e62f
  1 spec:
11    clusterIP: 10.96.0.0
  1   clusterIPs:
  2   - 10.96.0.0 
  3   externalTrafficPolicy: Cluster
  4   internalTrafficPolicy: Cluster
  5   ipFamilies:
  6   - IPv4
  7   ipFamilyPolicy: SingleStack
  8   ports:
  9   - nodePort: 32769
 10     port: 80
 11     protocol: TCP
 12     targetPort: 80

```

#### ***D***elete 7 lines ***j***umping down, including the current line
```
d7j          # 'd'elete 7 lines 'j'umping down
```

The unwanted lines should now be gone:
```yaml
 10 apiVersion: v1
  9 kind: Service
  8 metadata:
  7   creationTimestamp: "2025-01-14T18:27:29Z"
  6   labels:
  5     run: nginxpod
  4   name: nginx-service
  3   namespace: default
  2   uid: 0d175654-b5d4-4306-85c3-924c6f64e62f
  1 spec:
11    ports:
  1   - nodePort: 32769
  2     port: 80
  3     protocol: TCP
  4     targetPort: 80
...
```

With the unwanted lines deleted we can now apply the service yaml.
#### Save and apply the yaml - again - without leaving Vim!
```
ESC                           # 'ESC'ape from INSERT mode
:w nginx-service.yaml         # Save the YAML definition
:!kubectl apply -f %          # Run `kubectl apply` directly from inside Vim
```
#### If you have already set up the bash variable / shortcut `ka` in the previous exercises use it like this:
```
ESC                           # 'ESC'ape from INSERT mode
:w nginx-service.yaml         # Save the YAML definition
:!$ka -f %                    # Run `kubectl apply` directly from inside Vim
```

#### If everything is fine with the yaml definition, nginx-service.yaml gets applied, service is created.
```
k get svc -o wide             # view the service, -o wide shows more info about resources
```

#### Don't forget to define a shortcut for `--dry-run=server -o yaml` in your `.bashrc`:
```
vim ~/.bashrc
G                                       # Go to the last line
i                                       # Switch to INSERT mode
export drys='--dry-run=server -o yaml'  # use your own shortcut, if you don't like 'drys'
```

#### ***w***rite (save), ***q***uit, ***source*** (reload) the `.bashrc`
```
ESC
:wq
source ~/.bashrc
```
#### Congratulations!
You have learned how to  use `--dry-run=server` to expose a pod with a NodePort service and how to delete unwanted lines in YAML definitions using fast navigation by line numbers. Practice, practice, practice...

Next: [kubectl for CKAD - exercise 10 - copy from browser, paste into Vim, indent](https://miroberes.github.io/CKAD-Exam-Tips-kubectl-exercises/CKAD-Exam-Tips-kubectl-exercises-010-copy-from-browser-to-vim-indent.html)
