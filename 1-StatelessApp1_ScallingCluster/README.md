>Apply the new YAML file:
```
 $ kubectl apply -f deployment-update.yaml
```

 Note: By changing yaml file could change application like,
 1) Change number of running pods (Scalling replica count)
 2) Chnaging docker image
 Only has to do is change yaml file and run above command again

 >Watch the deployment create pods with new names and delete the old pods:

```
    $ kubectl get pods -l app=nginx
```