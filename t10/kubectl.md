https://www.udemy.com/course/kubernetesmastery/learn/lecture/17070896#overview
https://kubernetes.io/docs/reference/#api-reference

# Basic kubectl
## kubectl get
kubectl get nodes

kubectl get nodes -o wide

kubectl get nodes -o yaml

kubectl get nodes -o json

kubectl get nodes -o json | jq ".items[] | {name:.metadata.name} + .status.capacity"

- `| jq`: This is a pipe to the `jq` command, which will process the JSON output from the previous command.

- `".items[] | {name:.metadata.name} + .status.capacity"`: This is the `jq` filter that processes the JSON.

Here's what the filter does:

- `.items[]`: This part navigates to the "items" array in the JSON, which contains information about each node. It will apply the rest of the filter to each item in the array (each node).

- `{name:.metadata.name}`: This creates a new JSON object for each node with a single property "name", which is set to the value of the "name" property in the "metadata" object of each node.

- `+ .status.capacity`: This is adding (combining) the "capacity" object from the "status" object of each

## kubectl describe
kubectl describe node/<node>

kubectl describe node <node>

## kubectl api-resource
list all resources, following with `kubectl get` or 
kubectl api-resource

## kubectl explain
view definition of resource
kubectl explain <type>
kubectl explain node.spec

kubectl explain node --recursive


# kubectl get: list running containers
kubectl get services
kubectl get svc
kubectl get pod
	No pod found

kubectl get ns
kubectl get pods --all-namespaces
kubectl get pods -A
kubectl get pods -n <namespace>


# kubectl deploy a simple image

## create with kubectl run 
williamwang@Devops ~/devops/github/votingapp % kubectl run pingpong --image alpine ping 1.1.1.1
pod/pingpong created
williamwang@Devops ~/devops/github/votingapp % 
williamwang@Devops ~/devops/github/votingapp % kubectl get all
   get all kind of resources, pod, service, deploy etc.


williamwang@Devops ~/devops/github/votingapp % kubectl logs pingpong
64 bytes from 1.1.1.1: seq=435 ttl=61 time=19.956 ms

## create with deployment, replicas, and pod
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: pingpong
spec:
  replicas: 1
  selector:
    matchLabels:
      app: pingpong
  template:
    metadata:
      labels:
        app: pingpong
    spec:
      containers:
      - name: alpine
        image: alpine
        command: ['ping', '1.1.1.1']
```


williamwang@Devops ~/temp/k8s % kubectl create -f ping.yml 
deployment.apps/pingpong created

### check with get all, list all resource
williamwang@Devops ~/temp/k8s % kubectl get all
NAME                            READY   STATUS    RESTARTS   AGE
pod/pingpong                    1/1     Running   0          12m
pod/pingpong-6d6ff8754d-qwqll   1/1     Running   0          4s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   2d11h

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/pingpong   1/1     1            1           4s

NAME                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/pingpong-6d6ff8754d   1         1         1       4s
williamwang@Devops ~/temp/k8s % kubectl scale deployment.apps/pingpong --replicas 3
deployment.apps/pingpong scaled
williamwang@Devops ~/temp/k8s % 
williamwang@Devops ~/temp/k8s % 
williamwang@Devops ~/temp/k8s % kubectl get all                                    
NAME                            READY   STATUS              RESTARTS   AGE
pod/pingpong                    1/1     Running             0          12m
pod/pingpong-6d6ff8754d-dzgp7   0/1     ContainerCreating   0          4s
pod/pingpong-6d6ff8754d-qwqll   1/1     Running             0          55s
pod/pingpong-6d6ff8754d-rpfgx   1/1     Running             0          4s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   2d11h

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/pingpong   2/3     3            2           55s

NAME                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/pingpong-6d6ff8754d   3         3         2       55s


### get recent log
williamwang@Devops ~/temp/k8s % kubectl logs deployment.apps/pingpong --tail 1 --follow
Found 3 pods, using pod/pingpong-6d6ff8754d-qwqll
64 bytes from 1.1.1.1: seq=160 ttl=61 time=20.729 ms
64 bytes from 1.1.1.1: seq=161 ttl=61 time=24.714 ms


## terminiate a pod in replicas

Graceful terminal

### screen one
williamwang@Devops ~/temp/k8s % kubectl logs deployment.apps/pingpong --tail 1 --follow
Found 3 pods, using pod/pingpong-6d6ff8754d-rpfgx
64 bytes from 1.1.1.1: seq=897 ttl=61 time=19.801 ms

### screen two
williamwang@Devops ~/temp/k8s % watch kubectl get pods
Every 2.0s: kubectl get pods                                                                                devops.lan: Sat Oct 21 08:44:12 2023

NAME                        READY   STATUS    RESTARTS   AGE
pingpong                    1/1     Running   0          29m
pingpong-6d6ff8754d-dsdbx   1/1     Running   0          66s
pingpong-6d6ff8754d-dzgp7   1/1     Running   0          16m
pingpong-6d6ff8754d-tl5zw   1/1     Running   0          3m58s

### screen three
williamwang@Devops ~/temp/k8s % kubectl delete pod/pingpong-6d6ff8754d-rpfgx
pod "pingpong-6d6ff8754d-rpfgx" deleted
williamwang@Devops ~/temp/k8s % 


## Cronjob
https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/


williamwang@Devops ~/temp/k8s % kubectl apply -f cron.yml 
cronjob.batch/hello created
williamwang@Devops ~/temp/k8s % kubectl get cronjobs 
NAME    SCHEDULE    SUSPEND   ACTIVE   LAST SCHEDULE   AGE
hello   * * * * *   False     0        <none>          26s
williamwang@Devops ~/temp/k8s % kubectl get jobs    
NAME             COMPLETIONS   DURATION   AGE
hello-28297686   1/1           9s         56s
williamwang@Devops ~/temp/k8s % cat cron.yml 
apiVersion: batch/v1
kind: CronJob
metadata:
  name: hello
spec:
  schedule: "* * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: hello
            image: busybox:1.28
            imagePullPolicy: IfNotPresent
            command:
            - /bin/sh
            - -c
            - date; echo Hello from the Kubernetes cluster
          restartPolicy: OnFailure
williamwang@Devops ~/temp/k8s %



###  get log from multiple pods 

williamwang@Devops ~/temp/k8s % stern pingpong
pingpong-6d6ff8754d-tl5zw alpine 64 bytes from 1.1.1.1: seq=4227 ttl=61 time=18.632 ms
pingpong-6d6ff8754d-dsdbx alpine 64 bytes from 1.1.1.1: seq=4055 ttl=61 time=24.189 ms
pingpong-6d6ff8754d-dzgp7 alpine 64 bytes from 1.1.1.1: seq=4956 ttl=61 time=20.837 ms
^C
williamwang@Devops ~/temp/k8s % stern --tail 1 --timestamps pingpong
+ pingpong-6d6ff8754d-dsdbx › alpine
+ pingpong-6d6ff8754d-dzgp7 › alpine
+ pingpong-6d6ff8754d-tl5zw › alpine
pingpong-6d6ff8754d-tl5zw alpine 2023-10-21T15:32:31.336990514+11:00 64 bytes from 1.1.1.1: seq=4279 ttl=61 time=17.784 ms
pingpong-6d6ff8754d-dzgp7 alpine 2023-10-21T15:32:31.656836708+11:00 64 bytes from 1.1.1.1: seq=5008 ttl=61 time=22.058 ms















