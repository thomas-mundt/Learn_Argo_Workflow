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



## Create a workflow


```
git clone https://github.com/argoproj/argo-workflows.git
cd argo-workflows

k -n argo create -f examples/hello-world.yaml
```


vi examples/hello-world.yaml
```
apiVersion: argoproj.io/v1alpha1
kind: Workflow
metadata:
  generateName: hello-world-
  labels:
    workflows.argoproj.io/archive-strategy: "false"
  annotations:
    workflows.argoproj.io/description: |
      This is a simple hello world example.
      You can also run it in Python: https://couler-proj.github.io/couler/examples/#hello-world
spec:
  entrypoint: whalesay
  templates:
  - name: whalesay
    container:
      image: docker/whalesay:latest
      command: [cowsay]
      args: ["hello world"]
```




