# Kind start cluster with one worker and exposed ports
```console
kind create cluster --config fosdem2022_contour_kind.yaml
```
# verify all pods are running
```console
kubectl get pods -A 
```
# Deploy Contour
```console
kubectl apply -f https://projectcontour.io/quickstart/contour.yaml

kubectl get pods -A
```

# 1. Deploy simple web server under local.projectcontour.io
```console
kubectl apply -f 01-main.yaml
```

# 2. Create HTTPProxy for local.projectcontour.io
```
kubectl apply -f 02-proxy-main.yaml
```

# In second terminal
```
while true; do curl -s local.projectcontour.io|grep FOSDEM ; sleep 0.2 ; done
```
# 3. Create fosdem namespace and deployments
```
kubectl apply -f 02-proxy-main.yaml
```
