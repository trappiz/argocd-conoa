# Kubernetes cluster
## Using minikube to make things as simple as possible in a demo envirnoment

minikube start --cpus 4 --memory 16384

## Enable metallb for loadbalancer locally
minikube addons enable metallb

## add lb ips
minikube addons configure metallb
192.168.39.110
192.168.39.120

## ArgoCD Install
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

## Get init admin password
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo

## Port forward to access webgui for now
kubectl port-forward svc/argocd-server -n argocd 8080:443


## Install helm app 
## In this case we will install nginx

Create a new app with name and namespace ingress-nginx
Point repository to https://kubernetes.github.io/ingress-nginx
Set helm chart version 4.4.0

Sync!

This is the same as running:

```bash
#helm upgrade --install ingress-nginx ingress-nginx \
#  --repo https://kubernetes.github.io/ingress-nginx \
#  --namespace ingress-nginx --create-namespace
```

## Create argocd ingress
argocd-ingress.yaml

## Add github repository to argocd
1. Settings
2. Repositories
3. Connect repo
4.1. Select "via https"
4.2. Type: git
4.2. Enter repository url: https://github.com/trappiz/argocd-conoa

## Deploy podinfo demo app from github repo using kustomizaton
Create a new app in ArgoCD
1. Applications
2. New app
3. Enter name, url, ns..
4. Sync!

## Podinfo url
https://podinfo.home.lab

## Lets change the deployment! Change branch for podinfo
1. Click app
2. Edit
3. Change branch from master to demo
4. Sync!
