# optimizer-k8s
ArgoCD declarative pull based Gitops repository representing the state of the paint batch kubernetes cluster 

## optimizer-k8s contains the following ArgoCD applications

 - metrics-server - needed for the horizontal pod scaler and when prometheus is added for scraping
 - argocd
 - parent application - once bootstrapped this is used to add new applications and sync changes to other application definitions without needing to apply them in


### Prerequisites
1. `brew install kubernetes-cli`
2. `brew install kubernetes-helm`

### bootstrap cluster
These steps can be automated.

1. Create a cluster manually on your preferred cloud provider.
2. Fetch and apply in argo cd
  ```
  helm repo add argo https://argoproj.github.io/argo-helm
  helm fetch argo/argo-cd --untar
  cd ./argo-cd
  kubectl create namespace argocd
  helm --namespace argocd --set server.extraArgs={--insecure} template . | kubectl apply -n argocd -f -
  ```
3. Apply in the parent ArgoCD application
```
kubectl apply -n argocd -f bootstrap
```

4. Sync ArgoCD applications

5. For bots and snooker

`kubectl create secret docker-registry regcred --docker-server=https://index.docker.io/v1/ --docker-username=$DOCKER_USERNAME --docker-password=$DOCKER_PASSWORD --docker-email=$DOCKER_EMAIL -n bots`

`kubectl create secret docker-registry regcred --docker-server=https://index.docker.io/v1/ --docker-username=$DOCKER_USERNAME --docker-password=$DOCKER_PASSWORD --docker-email=$DOCKER_EMAIL -n snooker`

6. For soccer goals bot
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: soccer-goals-bot
data:
  BOT_TOKEN:
  GA_TRACKING_ID:
  FIRE_BASE_APP:
````

# TODO:
 * Add external-dns to make dns registering declarative
 * Add [chaos monkey](https://github.com/helm/charts/tree/master/stable/chaoskube) to introduce controlled failure into the live cluster. This is not useful until monitoring is present.