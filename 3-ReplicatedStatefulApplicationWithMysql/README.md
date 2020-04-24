Sample app for how to run a replicated stateful application using a StatefulSet controller. The example is a MySQL single-master topology with multiple slaves running asynchronous replication.

## Deploy MySQL
>The mysql-configmap.yaml, MySQL deployment consists of a ConfigMap, two Services, and a StatefulSet.

```
    $ kubectl apply -f mysql-configmap.yaml
```

This ConfigMap provides my.cnf overrides that let you independently control configuration on the MySQL master and slaves. In this case, you want the master to be able to serve replication logs to slaves and you want slaves to reject any writes that don’t come via replication.

There’s nothing special about the ConfigMap itself that causes different portions to apply to different Pods. Each Pod decides which portion to look at as it’s initializing, based on information provided by the StatefulSet controller.

> Services
Create the Services from the mysql-services.yaml configuration file:
```
    $ kubectl apply -f mysql-services.yaml
```

The Headless Service provides a home for the DNS entries that the StatefulSet controller creates for each Pod that’s part of the set. Because the Headless Service is named mysql, the Pods are accessible by resolving ``` <pod-name> ```.mysql from within any other Pod in the same Kubernetes cluster and namespace.

The Client Service, called mysql-read, is a normal Service with its own cluster IP that distributes connections across all MySQL Pods that report being Ready. The set of potential endpoints includes the MySQL master and all slaves.

Note that only read queries can use the load-balanced Client Service. Because there is only one MySQL master, clients should connect directly to the MySQL master Pod (through its DNS entry within the Headless Service) to execute writes.




