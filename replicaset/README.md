# Replicaset

`replicaset` supervises
`spec.selector.matchLabels` and `spec.templdate.metadata.labels` should be matched.

```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
	...
spec:
  template:
    metadata:
      labels:
        app: nginx-replicaset
    spec:
      containers:
	...
  replicas: 3
  selector:
    matchLabels:
      app: nginx-replicaset
```

```
kubectl apply -f replicaset-nginx.yaml
kubectl get pods --show-labels
kubectl exec -ti nginx-replicaset-[search through kubectl get pods] /bin/sh
exit
```

although we delete one of pods, replicaset supervise it and recreate another pod to keep 3 replicas.
```
kubectl delete pod nginx-replicaset-[one of pods]
kubectl get pods --show-labels
```

check `replicaset` and pods.

```
kubectl get replicasets,pods
```

delete replicaset except for pods using `--cascade=false`.

```
kubectl delete replicaset nginx-replicaset --cascade=false
kubectl get replicasets,pods
kubectl delete pod nginx-replicaset-[one of pods]
kubectl get pods --show-labels
``` 

```
tony_yoo@cloudshell:~/kubernetes-book-sample/replicaset (k8s-tony-01)$ kubectl get replicasets,pods --show-labels
NAME                         READY   STATUS    RESTARTS   AGE   LABELS
pod/nginx-replicaset-t9ppf   1/1     Running   0          29m   app=nginx-replicaset
pod/nginx-replicaset-xg7pr   1/1     Running   0          29m   app=nginx-replicaset
```

Now, reset replicaset to revive 3 replicas.

```
kubectl apply -f replicaset-nginx.yaml 
```

Let's edit `labels.app` for a pod of replicas.

```
kubectl edit pod nginx-replicaset-[select a pod]
# change nginx-replicaset to nginx-other in the metadat.labels.app field.
```

And then check it as follows.

```
tony_yoo@cloudshell:~/kubernetes-book-sample/replicaset (k8s-tony-01)$ kubectl get replicasets,pods --show-labels
NAME                               DESIRED   CURRENT   READY   AGE     LABELS
replicaset.apps/nginx-replicaset   3         3         3       3m13s   <none>

NAME                         READY   STATUS    RESTARTS   AGE     LABELS
pod/nginx-replicaset-27v55   1/1     Running   0          13s     app=nginx-replicaset
pod/nginx-replicaset-994ss   1/1     Running   0          3m13s   app=nginx-other
pod/nginx-replicaset-t9ppf   1/1     Running   0          32m     app=nginx-replicaset
pod/nginx-replicaset-xg7pr   1/1     Running   0          32m     app=nginx-replicaset
```

delete all pods. 
```
kubectl delete -f replicaset-nginx.yaml
kubectl delete pod nginx-replicaset-994ss
```
