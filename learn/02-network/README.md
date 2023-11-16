# Lab Network
## apply lab cluster
1. apply kind from kind-lab-network
2. create namespace app-dev
```
kubectl create ns app-dev
```

## nodeport service
1. run nodeport service and app deployment
```
kubectl apply -f nodeport.yaml -n app-dev
```
2. test nodeport by open browser and browser http://localhost:30001 or http://localhost:30011 (port of simultion nodes)
3. test connect service inside cluster
```
# test from same namespace
kubectl run -it --rm test -n app-dev --image=alpine/curl -- sh
$ curl http://backend-service

# test from difference namespace
kubectl run -it --rm test  --image=alpine/curl -- sh
$ curl http://backend-service

$ curl http://backend-service.app-dev
```
4. rm
```
kubectl delete -f nodeport.yaml -n app-dev
```

## clustreIP service
1. run nodeport service and app deployment
```
kubectl apply -f clusterip.yaml -n app-dev
```
2. test connect service inside cluster
```
# test from same namespace
kubectl run -it --rm test -n app-dev --image=alpine/curl -- sh
$ curl http://backend-service

# test from difference namespace
kubectl run -it --rm test --image=alpine/curl -- sh
$ curl http://backend-service

$ curl http://backend-service.app-dev
```
3. rm
```
kubectl delete -f clusterip.yaml -n app-dev
```

