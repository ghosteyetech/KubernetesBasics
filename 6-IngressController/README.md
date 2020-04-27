# Ingress
Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the Ingress resource.

> Enable addon ingress
```
    $ minikube addons enable ingress
```
### NOTE : wait a min for the pod to be up and running
```
    $ kubectl get pods -n kube-system | grep nginx-ingress-controller
```

> Create deployment
```
    $ kubectl apply -f wildflyapp1-deployment.yaml
    $ kubectl apply -f wildflyapp2-deployment.yaml
```
### wait a min for the deployment to be created
```
    $ kubectl get deploy
```

> use command kubectl get pods to check , it should be like this
```
    $ kubectl get pods
```

> Watching pogress of pod by app name
```
    $ kubectl get pods -l app=wn-sass-web-portal --watch
```

### Create service connecting deployment (Only if not specify in the yml file)
```
    $ kubectl expose deployment wn-sass-web-portal --target-port=8080 --type=NodePort
    $ kubectl expose deployment wildfly-app-healthcheck --target-port=8080 --type=NodePort
```

> use kubectl get services to check,
```
    $ kubectl get svc
```

>Let’s use curl to see if we can get data from the wn-sass-web-portal application’s healthcheck endpoint:
```
    $ curl $(minikube service wn-sass-web-portal --url)/WNSassWebPortal
    $ curl $(minikube service wildfly-app-healthcheck --url)/WildFlyAppWithHealthCheck
```

>To find out the exposed IP and Port inside the cluster we can use describe service:
```
    $ kubectl describe services/wn-sass-web-portal
    $ kubectl describe services/wildfly-app-healthcheck
```

> Get description of pods with events
```
    $ kubectl describe pods
    or
    $ kubectl describe pod wn-sass-web-portal
```

## Create Ingress controller
<!-- Ingress resources needed a Ingress Controller to handle it. In here, I will use the implementation of nginx as example
```
    $ kubectl run nginx --image=nginx --port=80
```

> Note: you can specify the default node in backend, and route to different node by the path in rules, for more please check the https://kubernetes.io/docs/concepts/services-networking/ingress/

> next, expose it to each node
```
    $ kubectl expose deployment nginx --target-port=80 --type=NodePort
``` -->

> Setup Ingress Resource and apply
```
    $ kubectl apply -f ingress-config.yaml
```

> it would expose an external ip, you can check the resource by
```
    $ kubectl get ing
```
from here `<ADDRESS>` will be the external ip which is public accessible.

and you can use kubectl describe ingress test-node-adv-ingress to view its behavior, like this:
```
    $ kubectl describe ingress test-node-adv-ingress
```

> PLEASE NOTE THAT: it may take 5–8 minutes for the node to work with ingress, please be patient. and also you may need to set up firewall rules in gcloud to allow traffic, please refer to the google cloud doc.
> NOTE 2: the ingress needed a root url (‘/’) send 200 ok to work

## Edit ingress config 
```
    $ kubectl edit ingress test-node-adv-ingress
```

## Accessing your application
``` 
    $ minikube ip
    $ curl $(minikube ip)/WildFlyAppWithHealthCheck 
    $ curl $(minikube ip)/WNSassWebPortal
```

## Cleanup

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
    $ kubectl delete services wn-sass-web-portal
    $ kubectl delete services wildfly-app-healthcheck
```

>Delete the deployment by name:
```
    $ kubectl delete deployment wn-sass-web-portal
    $ kubectl delete deployment wildfly-app-healthcheck
```

Delete pods:
```
    $ kubectl delete pod <podname>
```

Allow pods back onto nodes
```
    $ kubectl uncordon <nodename>
```