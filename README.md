# optimizer-k8s
ArgoCD declarative pull based Gitops repository representing the state of the paint batch kubernetes cluster 

## optimizer-k8s contains the following ArgoCD applications

 - paint-batch-optimizer
 - metrics-server - needed for the horizontal pol scaler
 - argocd


### Prerequisites
1. `brew install kubernetes-cli`
2. `brew install kubernetes-helm`

### bootstrap cluster
These steps can be automated.

1. Create a cluster manually on your preferred cloud provider.
2. Apply ArgoCD helm chart into the kubernetes cluster  
  ```
  helm template charts/argocd | kubectl apply -n argocd -f -
  ```
3. Apply in the parent ArgoCD application
```
kubectl apply -n argocd -f bootstrap
```

4. Sync ArgoCD applications

# TODO:
 * Add external-dns to make dns registering declarative
 * Add observability with prometheus, alert manager and Grafana
 * Add reverse proxy like ambassador or Istio to monitor North/South and East/West traffic and to reduce the number of Load Balancers.
 * Add [chaos monkey](https://github.com/helm/charts/tree/master/stable/chaoskube) to introduce controlled failure into the live cluster. This is not useful until monitoring is present.