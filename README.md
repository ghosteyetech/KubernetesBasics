# QubernetesBasics

## Minikube
Check that it is properly installed, by running the minikube version command:
```
$ minikube version
```

Start the cluster, by running the minikube start command:
```
$ minikube start --driver=virtualbox
```

## kubectl
After starting Minkube, To check if kubectl is installed you can run the kubectl version command:
```
$ kubectl version
```

## Cluster details
Let’s view the cluster details. We’ll do that by running kubectl cluster-info:
```
$ kubectl cluster-info
```

To view the nodes in the cluster, run the kubectl get nodes command:
```
$ kubectl get nodes
```

This command shows all nodes that can be used to host our applications. Now we have only one node, and we can see that its status is ready (it is ready to accept applications for deployment).

## Using kubectl to Create a Deployment

>create a Kubernetes Deployment using an existing image named echoserver, which is a simple HTTP server and expose it on port 8080 using --port.
```
    $ kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.10
```

>To access the hello-minikube Deployment, expose it as a Service:

```
    $ kubectl expose deployment hello-minikube --type=NodePort --port=8080
```

>The hello-minikube Pod is now launched but you have to wait until the Pod is up before accessing it via the exposed Service.

>Check if the Pod is up and running:

```
    $ kubectl get pod
```

>Get all running services

```
    $ kubectl get --all-namespaces services
```

>Similarly, you can take a look at the set of pods that were created during cluster startup.

```
    $ kubectl get --all-namespaces pods
```

Note: If the output shows the STATUS as ContainerCreating, the Pod is still being created.

>If the output shows the STATUS as Running, the Pod is now up and running.

> Get the URL of the exposed Service to view the Service details:

```
    $ minikube service hello-minikube --url
```

>Delete the hello-minikube Service:

```
    $ kubectl delete services hello-minikube
```

>Delete the hello-minikube Deployment:

```
    $ kubectl delete deployment hello-minikube
```

>Stop the local Minikube cluster:

```
    $ minikube stop
```
>Delete the local Minikube cluster:

```
    $ minikube delete
```

>To access the Kubernetes Dashboard, run this command in a shell after starting Minikube to get the address:

```
    $ minikube dashboard
```

## Enable addons
Minikube has a set of built-in addons that can be enabled, disabled and opened in the local Kubernetes environment.

> List the currently supported addons:
```
    $ minikube addons list
```

> Enable an addon, for example, metrics-server:
```
    $ minikube addons enable metrics-server
```

> View the Pod and Service you just created:
```
    $ kubectl get pod,svc -n kube-system
```

> Disable metrics-server:
```
    $ minikube addons disable metrics-server
```




