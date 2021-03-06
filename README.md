Argo CD
===

# Install

```
kubectl create namespace argocd
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

### Affinity & tolerations

```
affinity:
  nodeAffinity:
    preferredDuringSchedulingIgnoredDuringExecution:
    - weight: 1
      preference:
        matchExpressions:
        - key: cloud.google.com/gke-nodepool
          operator: In
          values:
          - preemptible
tolerations:
- key: "preemptible"
  operator: "Equal"
  value: "true"
  effect: "PreferNoSchedule"
```

### (GCP) add ip-masq-agent

[Check ip masquerade agent](deploy/ip-masq-agent/README.md)

# Access

Init password
```
SERVER_HOST=argocd.chechia.net
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

---

# App

Create app
```
APP=argocd
REPOSITORY=
MANEFEST_PATH=
argocd app create ${APP} \
  --repo ${REPOSITORY} \
  --path ${MANAFEST_PATH} \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace default

argocd app get ${APP}
argocd app sync ${APP}
```

# Cluster

```
argocd cluster add
CONTEXT=
argocd cluster add ${CONTEXT}
```

Check firewall rules to make sure the remote cluster is reachable
