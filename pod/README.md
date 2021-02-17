# Pod

docker image: [simple-container-app](https://hub.docker.com/layers/arisu1000/simple-container-app/latest/images/sha256-18f5a0fb9d1faf26862eb7a301b5c2a8debe80f60e5db03ddeb16977d3c76011?context=explore)


## pod-simple

```
kubectl apply -f pod-simple.yaml
kubectl exec -ti kubernetes-simple-pod /bin/sh
```

> Question: what is the difference between bin/bash and bin/sh? see [the answer](https://storycompiler.tistory.com/101)

In the container, check the os info.
```
cat /etc/os-release
```

```
kubectl delete -f pod-simple.yaml
```

## pod-all

set resources and env variables and check it.

```
kubectl apply -f pod-all.yaml
kubectl exec -ti kubernetes-simple-pod /bin/sh
```

Let's see the `env`.

```
~ # env
POD_IP=10.112.2.9
KUBERNETES_SERVICE_PORT=443
KUBERNETES_PORT=tcp://10.116.0.1:443
CPU_REQUEST=1
HOSTNAME=gke-k8s-cluster-default-pool-8c4e34bc-4w6t
TESTENV01=testvalue01
SHLVL=1
HOME=/root
TERM=xterm
POD_NAME=kubernetes-simple-pod
KUBERNETES_PORT_443_TCP_ADDR=10.116.0.1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
KUBERNETES_PORT_443_TCP_PORT=443
KUBERNETES_PORT_443_TCP_PROTO=tcp
CPU_LIMIT=1
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_PORT_443_TCP=tcp://10.116.0.1:443
KUBERNETES_SERVICE_HOST=10.116.0.1
PWD=/root
```

delete the pod.
```
kubectl delete -f pod-all.yaml
```
