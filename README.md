# argocd-deploys-demo

ArgoCD Managed via ArgoCD


![inception meme :v](https://i.imgflip.com/80tp51.jpg)


# prerequisites

Before you begin with this demo, make sure you have the following:

* A Kubernetes cluster (such as minikube,kind, k3s, or any cloud provider's Kubernetes alternative).
* github account
* ssh key pair for ArgoCD authentication.
* Helm installed.



## create argo namespace
While in a real-world scenario, setting up the Argo namespace might be part of the cluster's bootstrap process, for demonstration purposes, we'll create it manually:

```
kubectl create ns argo
```

## install argocd server helm chart

Typically, resources like ArgoCD are managed through a CI/CD pipeline. In this case, ArgoCD is installed via Helm. Keep in mind that one ArgoCD server can often serve multiple clusters, but for this demo, we use a single server:

```
helm upgrade --install argocd . -n argo

example for loading ssh-key
helm upgrade -i -n argo argo-cd . --create-namespace \
   --set gitRepo.sshPrivateKey="`cat /path/to/priv/key`" \
   
```


# check argocd server  


### retrieve admin user/password

```
echo "Password: $(kubectl -n argo get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d)"
```
## port-forwarding argocd server

For web access to the ArgoCD server over HTTP:


```

kubectl port-forward -n argo svc/argocd-argocd-server 8080:8080

```
For HTTPS (SSL) access:
```
kubectl port-forward -n argo svc/argocd-argocd-server 8443:443

```
Note: This demo uses non-SSL mode for simplicity.

## install arcgcd-deploy helm chart

This step sets up the bootstrap configuration for each cluster/environment that will use ArgoCD. After the initial install, ArgoCD will take care of the rest:

```
helm upgrade --install argocd-deploy-demo . -f values/product1/default.yaml  -f values/product1/env/demo.yaml -n argo
```

## modify services deployed to cluster

You can use the ArgoCD-Deploy chart abstraction to add all the services required on clusters, and ArgoCD will automatically handle them.

default.yaml
```
applications:
  # enabled: true
  argo:
    service-example:
      enabled: false
      repo: git@github.com/jrab66/argocd-deploys-demo
      branch: main
      path: nginx/chart
      valueFiles:
        - values.yaml

```

## enjoy

This demo showcases ArgoCD's capabilities to manage and deploy complex projects seamlessly, making it a powerful tool for Kubernetes deployments and GitOps workflows.