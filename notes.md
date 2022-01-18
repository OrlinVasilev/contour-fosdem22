# Kind

 $ kind create cluster --config fosdem2022_contour_kind.yaml

kubectl get pods -A #verify all pods are running

# Deploy Contour

 $ kubectl apply -f https://projectcontour.io/quickstart/contour.yaml

# deploy simple app

curl -O https://projectcontour.io/examples/kuard-httpproxy.yaml

kubectl apply -f kuard-httpproxy.yaml

# FOSDEM Setup Ingress

kubectl create ns fosdem

kubectl create deployment -n fosdem --image=nginx nginx

kubectl -n fosdem expose deployment nginx --port 80

kubectl -n fosdem create ingress nginx --rule="local.projectcontour.io/*=nginx:80"

# Deploy second service 

kubectl -n fosdem create deployment --image=httpd httpd

kubectl -n fosdem expose deployment httpd --port 80

kubectl apply -f weighted90-10.yaml


while true; do curl -s local.projectcontour.io | grep h1 ; sleep 0.1 ; done

tmux 
ctrl+B+%
ctrl+N