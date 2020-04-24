Tutorail Link: https://www.oreilly.com/content/how-to-manage-docker-containers-in-kubernetes-with-java/

> view all Services within Kubernetes
```
    $ kubectl get svc
```
>view all associated pods
```
    $ kubectl get pods
```

>Deploy service using wildflyapp-service.yaml
```
    $ kubectl apply -f wildflyapp-service.yaml
```
> Watching pogress of pod by app name
```
    $ kubectl get pods -l app=wn-sass-web-portal --watch
```

>Let’s use curl to see if we can get data from the wn-sass-web-portal application’s healthcheck endpoint:
```
    $ curl $(minikube service wn-sass-web-portal --url)/WNSassWebPortal
```

## Stoping and deleting pod
if multiple nodes running find specific node by using
```
    $ kubectl get nodes
    $ kubectl get pods -o wide | grep <nodename>
```

Mark the node as unschedulable by using the kubectl cordon command. This ensures that no new pods will get scheduled to the node while you are preparing it for removal or maintenance.
```
    $ kubectl cordon <nodename>
```

Delete the ConfigMap, Services, and PersistentVolumeClaims.
```
    $ kubectl delete configmap,service,pvc,rc -l app=wn-sass-web-portal
```

Delete pods:
```
    $ kubectl delete pod <podname>
```

Allow pods back onto nodes
```
    $ kubectl uncordon <nodename>
```