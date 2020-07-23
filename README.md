Argo CD
===

# Install

```
kubectl create namespace argocd
wget https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

kubectl apply -n argocd -f install.yaml

kubectl apply -n argocd -f ingress.yaml
```

# Access

```
SERVER_HOST=
PASSWORD=$(kubectl get pods -n argocd -l app.kubernetes.io/name=argocd-server -o name | cut -d'/' -f 2)

argocd login ${SERVER_HOST} --plaintext --grpc-web --username admin --password ${PASSWORD}
```
