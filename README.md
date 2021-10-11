# k8s_argocd
Deploy application to kubernetes using Argocd


Argo CD is a declarative, GitOps continuous delivery tool for Kubernetes.

## ArgoCD Advantages

* Argo is open source and designed for container workflow enginer.

* It can be integrated with multiple clusters and easy to deploy

* Automatic Synchronization with git and apply changes automatically or manual

### Install ArgoCD

```
kubectl create namespace argocd  # create namespace 
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### Download Argo CD CLI

brew install argocd

By default argo service type is ClusterIP to access argo dashboard externally service needs to be update to nodeport or loadbalancer

```kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}' ```

Get the default password

```kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d ```

Login into Argo CLI

argo login 

It will prompt for credentials. Default username is admin and enter the password that received  from secret

### Deploy application from CLI

* argocd app create demo --repo https://github.com/sathish-cs/k8s-argocd  --dest-namespace default --path . --dest-server https://kubernetes.default.svc


	* --repo - Provide the git repository where the code relies 

	* --dest-namespace - Provide namespace to deploy applications

	* --path - its required manifest file path. 

	* --dest-server - Cluster name where applications has to deploy. 

Get the clusters using the below command

	* argocd cluster list

* argocd app list - used to check the status of app. Status remains outofsync that needs to be synced to deploy application

* argocd app sync demo 

Sync policy can change to automatic to detect and deploy manifests automatically if any changes made on repository.


To check all the resources deployed in the application

* argocd app resources demo

In this case it shows deployments and services


