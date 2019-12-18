# optimizer-k8s
ArgoCD declarative pull based Gitops repository representing the state of the paint batch kubernetes cluster 

## optimizer-k8s contains the following ArgoCD applications

 - paint-batch-optimizer
 - metrics-server - needed for the horizontal pod scaler and when prometheus is added for scraping
 - argocd
 - parent application - once bootstrapped this is used to add new applications and sync changes to other application definitions without needing to apply them in


### Prerequisites
1. `brew install kubernetes-cli`
2. `brew install kubernetes-helm`

### bootstrap cluster
These steps can be automated.

1. Create a cluster manually on your preferred cloud provider.
2. Clone down ArgoCD chart
  ```
  git clone git@github.com:argoproj/argo-helm.git
  ```

2. Navigate to and Apply in ArgoCD helm chart into the kubernetes cluster  
  ```
  helm template --namespace argocd . | kubectl apply -n argocd -f -
  ```
3. Apply in the parent ArgoCD application
```
kubectl apply -n argocd -f bootstrap
```

4. Sync ArgoCD applications

# TODO:
 * Add external-dns to make dns registering declarative
 * Add [chaos monkey](https://github.com/helm/charts/tree/master/stable/chaoskube) to introduce controlled failure into the live cluster. This is not useful until monitoring is present.