# start kind cluster
## for service lab
```
kind create cluster --name lab-network --config=service-lab.yaml
```
## for ingress lab
```
kind create cluster --name lab-network --config=ingress-lab.yaml
## install nginx-ingress controller
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```
