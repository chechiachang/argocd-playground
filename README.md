Argo CD
===

# Install

```
kubectl create namespace argocd
wget -O deploy/install.yaml https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

kubectl apply -n argocd -f deploy/
```

### Disable tls

```
app.kubernetes.io/name: argocd-server
- command:
  - argocd-server
  - --staticassets
  - /shared/app
  - --insecure
```

# Access

Init password
```
SERVER_HOST=
PASSWORD=$(kubectl get pods -n argocd -l app.kubernetes.io/name=argocd-server -o name | cut -d'/' -f 2)

argocd login ${SERVER_HOST} --plaintext --grpc-web --username admin --password ${PASSWORD}
```

Update password
```
argocd account update-password

*** Enter current password:
*** Enter new password:
*** Confirm new password:
Password updated
```

#
