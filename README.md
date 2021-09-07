# Learn Argo Workflow


## Links 

- https://argoproj.github.io/argo-workflows/installation/


## Deploy 

```
k create ns argo
kubectl apply -n argo -f https://raw.githubusercontent.com/argoproj/argo-workflows/master/manifests/quick-start-postgres.yaml

# or later
kubectl apply -n argo -f https://github.com/argoproj/argo-workflows/releases/download/v3.1.9/install.yaml

```



Access the web UI
```
k port-forward deployment/argo-server 2746:2746 -n argo

https://localhost:2746
```

