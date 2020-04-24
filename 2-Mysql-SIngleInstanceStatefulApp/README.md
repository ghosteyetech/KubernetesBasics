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

