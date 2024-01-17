create ns
```
kubectl create ns app-dev
kubectl create ns app-prd
```
create sa
```
kubectl apply -f sa.yaml -n app-dev
```
binding to sa
```
kubectl apply -f role-rolebinding.yaml
```
run pod to test
```
kubectl run -ti debug --image=bibinwilson/docker-kubectl --overrides='{ "spec": { "serviceAccount": "app-dev" }  }' bash
```