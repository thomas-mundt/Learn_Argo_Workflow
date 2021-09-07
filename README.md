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
  generateName: hello-world-   # Name of this Workflow
  labels:
    workflows.argoproj.io/archive-strategy: "false"
  annotations:
    workflows.argoproj.io/description: |
      This is a simple hello world example.
      You can also run it in Python: https://couler-proj.github.io/couler/examples/#hello-world
spec:
  entrypoint: whalesay.   # Defining "whalesay" as the "main" template
  templates:
  - name: whalesay
    container:
      image: docker/whalesay:latest
      command: [cowsay]
      args: ["hello world"]
```


## Container Template

vi wf-container-template.yaml
```
apiVersion: argoproj.io/v1alpha1
kind: Workflow
metadata:
  generateName: wf-container-template-
spec:
  entrypoint: container-template
  templates:
  - name: container-template
    container:
      image: python:3.8-slim
      command: [echo, "The container template was executed successfully."]
```

```
k -n argo create -f wf-container-template.yaml
```



## Script Template

```
apiVersion: argoproj.io/v1alpha1
kind: Workflow
metadata:
  generateName: wf-script-template-
spec:
  entrypoint: script-template
  templates:
  - name: script-template
    script:
      image: python:3.8-slim
      command: [python]
      source: |
        print("The script template was executed successfully.")
```


```
k -n argo create -f wf-script-template.yaml
```


## Resource Template

vi wf-resource-template.yaml
```
apiVersion: argoproj.io/v1alpha1
kind: Workflow
metadata:
  generateName: wf-resource-template-
spec:
  entrypoint: resource-template
  templates:
  - name: resource-template
    resource:
      action: create
      manifest: |
        apiVersion: argoproj.io/v1alpha1
        kind: Workflow
        metadata:
          name: wf-test
        spec:
          entrypoint: test-template
          templates:
          - name: test-template
            script:
              image: python:3.8-slim
              command: [python]
              source: |
                print("Workflow wf-test created with resource template.")
```

```
k -n argo create -f wf-resource-template.yaml
```



















## Write logs to MinIO

```
k -n argo port-forward minio 9000:9000

#or like
k -n argo port-forward minio-58977b4b48-ds7s7 9000:9000

```


```
localhost:9000

admin
password
```


## Configuration

```
k describe cm -n argo workflow-controller-configmap
```


```
Data
====
artifactRepository:
----
s3:
  bucket: my-bucket
  endpoint: minio:9000
  insecure: true
  accessKeySecret:
    name: my-minio-cred
    key: accesskey
  secretKeySecret:
    name: my-minio-cred
    key: secretkey
...

AND MORE
```




## ARGOCLI

```
#https://argoproj.github.io/argo-cd/cli_installation/

brew install argocd

argocd version
```

