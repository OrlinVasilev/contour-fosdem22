# Demo setup and steps for Contour Session on Fosdem'22

## Kind start cluster with one worker and exposed ports
```console
kind create cluster --config fosdem2022_contour_kind.yaml
```
## verify all pods are running
```console
kubectl get pods -A
```
## Deploy Contour
```console
kubectl apply -f https://projectcontour.io/quickstart/contour.yaml

kubectl get pods -A
```

## Deploy simple web server under local.projectcontour.io
```console
kubectl apply -f 01-main.yaml
```

## Create HTTPProxy for local.projectcontour.io
```console
kubectl apply -f 02-proxy-main.yaml
```

### In second terminal
```console
while true; do curl -s local.projectcontour.io|grep FOSDEM ; sleep 0.2 ; done
```

## Create fosdem namespace and web-v1 deployments
```console
kubectl apply -f 03-fosdem.yaml
```

## Tune the root HTTPproxy to delagate /fosdem to local.projectcontour.io
```console
kubectl apply -f 04-proxy-include-fosdem.yaml
kubectl get httpproxy -A
```

### Change the curl to /fosdem so we can follow the canary deployment(or Blue/Green)
```console
while true; do curl -s local.projectcontour.io/fosdem|grep FOSDEM ; sleep 0.2 ; done
```

## Deploy web-v2 for fosdem
```console
kubectl apply -f 05-fosdem-v2.yaml
```

## Prep for canary or blue green
```console
kubectl apply -f 06-add-fosdem-v2.yaml
```

## Redistribiute the traffic 90/10%, 50/50 , 10/90 , 100% over to web-v2
```console
kubectl apply -f 07-90-10.yaml
kubectl apply -f 08-50-50.yaml
kubectl apply -f 09-10-90.yaml
kubectl apply -f 10-v2-only.yaml
```

## Rollback to web-v1
```console
kubectl apply -f 11-rollback.yaml
```

## Clean up
```console
kind delete clusters fosdem-contour-demo
```