# Deployment

> Prerequisite: 
> * what is pod? see `/pod`
> * what is controller/replicaset? see `/replicaset`

Deployment is a basic controller, which is used to deploy stateless applications in Kubernetes.

Deployment can control replicatset and operations like deploy, rollback and so on.

Check it out. (Please note that rc(replication control) is not generated.)
```
kubectl apply -f deployment-nginx.yaml
kubectl get deploy,rs,rc,pods
```

As you can see, `deployment.apps` creates `replicaset.apps`, which has hash key `6b4c4b9f48`.

Then, the `replicaset.apps/nginx-deployment-6b4c4b9f48` supervises 3 pods `pod/nginx-deployment-6b4c4b9f48-*`.

```
tony_yoo@cloudshell:~/kubernetes-book-sample/deployment (k8s-tony-01)$ kubectl get deploy,rs,rc,pods                       
NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-deployment   3/3     3            3           2m13s

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-deployment-6b4c4b9f48   3         3         3       2m13s

NAME                                    READY   STATUS    RESTARTS   AGE
pod/nginx-deployment-6b4c4b9f48-d4whh   1/1     Running   0          2m12s
pod/nginx-deployment-6b4c4b9f48-ffkk4   1/1     Running   0          2m12s
pod/nginx-deployment-6b4c4b9f48-v4q4j   1/1     Running   0          2m12s
```

For k8s, there are 3 ways of updating configuration settings.

* interative 
  * `kubectl set ...`: directly set the configuration.
  * `kubectl edit ...`: edit `yaml` option.
* statical
  * `vi *yaml`(edit what you want) and `kubectl apply -f *.yaml`.

`kubectl set` is used as follows.

`kubectl set image deployment/[the name of deployment] [the name of container]=[IMAGE:TAG]`.

```
kubectl set image deployment/nginx-deployment nginx-deployment=nginx:1.9.1
kubectl get deploy nginx-deployment -o=jsonpath="{.spec.template.spec.containers[0].image}{'\n'}"
# result
nginx:1.10.1
```

At this time, let's try edit way.

```
kubectl edit deploy nginx-deployment
```

And then, change the `spec.template.spec.containers[0].image` field.
```
...
template:
  ...
  spec:
    ...
    containers:
      ...
      - image: nginx: latest
```

Check it.
```
tony_yoo@cloudshell:~/kubernetes-book-sample/deployment (k8s-tony-01)$ kubectl get deploy nginx-deployment -o=jsonpath="{.s
pec.template.spec.containers[0].image}{'\n'}"
nginx:latest
``` 
