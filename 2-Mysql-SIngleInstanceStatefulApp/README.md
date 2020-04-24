# Objectives
1) Create a PersistentVolume referencing a disk in your environment.
2) Create a MySQL Deployment.
3) Expose MySQL to other pods in the cluster at a known DNS name.

>This mysql-pv.yaml file describes a Deployment that runs MySQL and references the PersistentVolumeClaim. The file defines a volume mount for /var/lib/mysql, and then creates a PersistentVolumeClaim that looks for a 5G volume. This claim is satisfied by any existing volume that meets the requirements, or by a dynamic provisioner.

>Note: The password is defined in the config yaml, and this is insecure. See Kubernetes Secrets for a secure solution.

Deploy the PV and PVC of the YAML file:
```
    $ kubectl apply -f mysql-pv.yaml
```
Deploy the contents of the YAML file:
```
    $ kubectl apply -f mysql-deployment.yaml
```
Display information about the Deployment:
```
    $ kubectl describe deployment mysql
```
List the pods created by the Deployment:
```
    $ kubectl get pods -l app=mysql
```
Inspect the PersistentVolumeClaim:
```
    $ kubectl describe pvc mysql-pv-claim
```

## Accessing the MySQL instance

The preceding YAML file creates a service that allows other Pods in the cluster to access the database. The Service option 'clusterIP: None' lets the Service DNS name resolve directly to the Pod’s IP address. This is optimal when you have only one Pod behind a Service and you don’t intend to increase the number of Pods.

>Run a MySQL client to connect to the server:
```
    $ kubectl run -it --rm --image=mysql:5.6 --restart=Never mysql-client -- mysql -h mysql -ppassword
```
This command creates a new Pod in the cluster running a MySQL client and connects it to the server through the Service. If it connects, you know your stateful MySQL database is up and running.

>Get mysql running service details(like ip):
```
    $ kubectl get pod mysql -o wide
```

## Delete the deployed objects by name:
```
    $ kubectl delete deployment,svc mysql
    $ kubectl delete pvc mysql-pv-claim
    $ kubectl delete pv mysql-pv-volume
```