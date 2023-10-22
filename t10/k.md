
# Basic kubectl
## kubectl get
```
kubectl get nodes
kubectl get nodes -o wide
kubectl get nodes -o yaml
kubectl get nodes -o json
kubectl get nodes -o json | jq ".items[] | {name:.metadata.name} + .status.capacity"
```
- `| jq`: This is a pipe to the `jq` command, which will process the JSON output from the previous command.

- `".items[] | {name:.metadata.name} + .status.capacity"`: This is the `jq` filter that processes the JSON.

Here's what the filter does:

- `.items[]`: This part navigates to the "items" array in the JSON, which contains information about each node. It will apply the rest of the filter to each item in the array (each node).

- `{name:.metadata.name}`: This creates a new JSON object for each node with a single property "name", which is set to the value of the "name" property in the "metadata" object of each node.

- `+ .status.capacity`: This is adding (combining) the "capacity" object from the "status" object of each

## kubectl describe
```
kubectl describe node/<node>
kubectl describe node <node>
```

## kubectl api-resource
list all resources
```
kubectl api-resource
```
You can check each resource  with `kubectl get <resource>` 

## kubectl explain
view definition of resource
```
kubectl explain <type>
kubectl explain node.spec
kubectl explain node --recursive
```

# kubectl get: list running containers
```
kubectl get services
kubectl get svc
kubectl get pod
	# No pod found

kubectl get ns
kubectl get pods --all-namespaces
kubectl get pods -A
kubectl get pods -n <namespace>
```

# kubectl deploy a simple image

## create with kubectl run 
```
% kubectl run pingpong --image alpine ping 1.1.1.1
pod/pingpong created
% kubectl get all
   # get all kind of resources, pod, service, deploy etc.
```
check log of this pod
```
% kubectl logs pingpong
64 bytes from 1.1.1.1: seq=435 ttl=61 time=19.956 ms
```

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

cli create deploy
```
% kubectl create -f ping.yml 
deployment.apps/pingpong created
```
### list all resource
check with get all
```
% kubectl get all
NAME                            READY   STATUS    RESTARTS   AGE
pod/pingpong                    1/1     Running   0          12m
pod/pingpong-6d6ff8754d-qwqll   1/1     Running   0          4s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   2d11h

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/pingpong   1/1     1            1           4s

NAME                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/pingpong-6d6ff8754d   1         1         1       4s

% kubectl scale deployment.apps/pingpong --replicas 3
deployment.apps/pingpong scaled

% kubectl get all                                    
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
```

### get recent log
```
% kubectl logs deployment.apps/pingpong --tail 1 --follow
Found 3 pods, using pod/pingpong-6d6ff8754d-qwqll
64 bytes from 1.1.1.1: seq=160 ttl=61 time=20.729 ms
64 bytes from 1.1.1.1: seq=161 ttl=61 time=24.714 ms
```

## terminiate a pod in replicas

Graceful terminal

### screen one
```
% kubectl logs deployment.apps/pingpong --tail 1 --follow
Found 3 pods, using pod/pingpong-6d6ff8754d-rpfgx
64 bytes from 1.1.1.1: seq=897 ttl=61 time=19.801 ms
```

### screen two
```
% watch kubectl get pods
Every 2.0s: kubectl get pods                                                                                devops.lan: Sat Oct 21 08:44:12 2023

NAME                        READY   STATUS    RESTARTS   AGE
pingpong                    1/1     Running   0          29m
pingpong-6d6ff8754d-dsdbx   1/1     Running   0          66s
pingpong-6d6ff8754d-dzgp7   1/1     Running   0          16m
pingpong-6d6ff8754d-tl5zw   1/1     Running   0          3m58s
```
### screen three
```
% kubectl delete pod/pingpong-6d6ff8754d-rpfgx
pod "pingpong-6d6ff8754d-rpfgx" deleted
```


## Cronjob
https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/

```
% kubectl apply -f cron.yml 
cronjob.batch/hello created

% kubectl get cronjobs 
NAME    SCHEDULE    SUSPEND   ACTIVE   LAST SCHEDULE   AGE
hello   * * * * *   False     0        <none>          26s

% kubectl get jobs    
NAME             COMPLETIONS   DURATION   AGE
hello-28297686   1/1           9s         56s

% cat cron.yml 
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
```



###  get log from multiple pods 

```
% stern pingpong
pingpong-6d6ff8754d-tl5zw alpine 64 bytes from 1.1.1.1: seq=4227 ttl=61 time=18.632 ms
pingpong-6d6ff8754d-dsdbx alpine 64 bytes from 1.1.1.1: seq=4055 ttl=61 time=24.189 ms
pingpong-6d6ff8754d-dzgp7 alpine 64 bytes from 1.1.1.1: seq=4956 ttl=61 time=20.837 ms
^C

% stern --tail 1 --timestamps pingpong
+ pingpong-6d6ff8754d-dsdbx › alpine
+ pingpong-6d6ff8754d-dzgp7 › alpine
+ pingpong-6d6ff8754d-tl5zw › alpine
pingpong-6d6ff8754d-tl5zw alpine 2023-10-21T15:32:31.336990514+11:00 64 bytes from 1.1.1.1: seq=4279 ttl=61 time=17.784 ms
pingpong-6d6ff8754d-dzgp7 alpine 2023-10-21T15:32:31.656836708+11:00 64 bytes from 1.1.1.1: seq=5008 ttl=61 time=22.058 ms
```


## Deploy process
- kubectl -> API server -> etcd (store Deployment resource)
- API server -> kubectl (get return result, "deployment.app/web created")
- controller manager check etcd via API server, that what is new resource in etcd datebase. Read the deployment resource, find there is ReplicaSet, write ReplicaSet resource to etcd.
- controller manager check etcd via API server, that what is new resource in etcd datebase. Read the ReplicaSet resource, find there are three Pod Pending required, write three pods resource resource to etcd.
- scheduler keep pull API server as well. Asking any new pods I need to schedule? scheduler decide which pod go to which node, and update it in etcd. pod pending --> pod nodeID
- Kubelet in Node is checking etcd via API server, that if any pod assigned to me.  pod nodeID --> pod creating; start creating pod; once started, update etcd: pod creating --> pod running.




### watch the process
#### window one
```
% kubectl create deploy web --image=nginx --replicas=3
deployment.apps/web created
```

#### Window two
```
% kubectl get pods -w
NAME                  READY   STATUS    RESTARTS   AGE
web-844f65fb5-8pdtw   1/1     Running   0          3h2m
web-844f65fb5-mp5mp   1/1     Running   0          3h2m
web-844f65fb5-z7dqh   1/1     Running   0          3h2m
web-844f65fb5-h6p7r   0/1     Pending   0          0s
web-844f65fb5-2knwg   0/1     Pending   0          0s
web-844f65fb5-h6p7r   0/1     Pending   0          0s
web-844f65fb5-2knwg   0/1     Pending   0          0s
web-844f65fb5-h6p7r   0/1     ContainerCreating   0          0s
web-844f65fb5-2knwg   0/1     ContainerCreating   0          0s
web-844f65fb5-h6p7r   1/1     Running             0          4s
web-844f65fb5-2knwg   1/1     Running             0          6s
```
#### Window 3
```
% watch kubectl get pods
Every 2.0s: kubectl...  devops.lan: Sat Oct 21 18:51:12 2023

NAME                  READY   STATUS    RESTARTS   AGE
web-844f65fb5-2knwg   1/1     Running   0          76s
web-844f65fb5-8pdtw   1/1     Running   0          3h5m
web-844f65fb5-h6p7r   1/1     Running   0          76s
web-844f65fb5-mp5mp   1/1     Running   0          3h5m
web-844f65fb5-z7dqh   1/1     Running   0          3h5m
```

## service Layer4
It's the pods that need to receive traffic. So, really what's happening is in IP tables, on our nodes, we're creating rules via the kube-proxy agent. It's going to direct traffic to the pods in a round robin fashion, by default, on Port 8888.
It's using the deployment as a selector for deciding which pods need to be inside this service.
That's why we're using the deployment here.

In a namespace, you can have the same name for different resource types, but you can't have a name collision of the combination of the resource type and the name of the resource.

IMPORTANT
Services are a Layer 4 concept (IP, protocal TCP/UDP, port).
Implementation of kube-proxy

## CoreDNS
https://coredns.io/
One of the jobs for a Service resource is to create the DNS name for the Service IP, which then makes something like httpenv resolvable on the Pod networks.
If you are using Docker Desktop or minikube, DNS is running out-of-the-box. All done :)

Apply vs Create + Edit


# Yaml file

## generate the yaml
```
% kubectl create deploy web --image nginx -o yaml --dry-run
% kubectl create namespace goodapp -o yaml --dry-run

```

## Start with hard
4 Parts
- apiVersion: v1
- kind: Namespace
- metadata:
- spec: {}

Find kind
```
kubectl api-resources
NAME                              SHORTNAMES   APIVERSION                             NAMESPACED   KIND
bindings                                       v1                                     true         Binding
componentstatuses                 cs           v1                                     false        ComponentStatus

```

Find apiVersion
```
% kubectl api-versions
admissionregistration.k8s.io/v1
apiextensions.k8s.io/v1

```

Find spec

```
williamwang@Devops ~ % kubectl explain pod
…
  spec	<PodSpec>
    Specification of the desired behavior of the pod. More info:
    https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status
…

% kubectl explain pod.spec
…

  volumes	<[]Volume>
    List of volumes that can be mounted by containers belonging to the pod. More
    info: https://kubernetes.io/docs/concepts/storage/volumes
…
% kubectl explain pod.spec.volumes

kubectl explain pod.spec --recursive

```

## kubectl diff
```
% kubectl run my-pod --image=nginx -o yaml --dry-run=client
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: my-pod
  name: my-pod
spec:
  containers:
  - image: nginx
    name: my-pod
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

 % kubectl run my-pod --image=nginx -o yaml --dry-run=client > mypod.yaml

kubectl apply -f mypod.yaml

vim mypod.yaml
Update to   - image: nginx:1.17


% kubectl diff -f mypod.yaml

```

