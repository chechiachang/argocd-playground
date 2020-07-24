IP Masquerage Agent
===

Use ip-masq-agent to masquerade source ip from pod ip to node ip will allow access to gitlab in private network.

# Why we need this

Use case: If your Pod requires access to internal service. But the CIDR for pods are not permitted by firewall. 

# Usage

Edit `deploy/ip-masq-agent/config`，comment cidr from default ranges to disable masquerade

```
kubectl create configmap ip-masq-agent \
  --from-file config \
  --namespace kube-system

kubectl apply -f ip-masq-agent.yaml
```
