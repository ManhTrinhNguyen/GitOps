## GitOps

### ArgoCD Installation

Go to `cd GitOps`
Step 1: Install Argocd in the Cluster 

- Create argocd namespace `kubectl create namespace argocd`
- Install argo cd `kubectl apply -n argocd --server-side --force-conflicts -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml`

Step 2 : Download Argocd CLI

- MacOS : `brew install argocd`

Step 3 : Access Argo CD by using port-forwarding

`kubectl port-forward svc/argocd-server -n argocd 8080:443`

Step 4 : Login in using CLI 

`argocd admin initial-password -n argocd`

### Argo CD Application 

Application is a CRD which represents a deployed applycation instance in a cluster 

- Source : is mapped to Git Repo where the desired state live of Kubernetes Manifest live 

- Destination: is a target where the resources should be deploy in a Kubernetes Cluster 

    - Project: default
    - Sync Policy : Automated option 
    - Ignore Diff: group | kind | Json pointer 

### Create Application using Argocd UI 

Go to `New App` -> Config name -> Config Project -> SynPolicy -> Source (Git Repo URL) ->  Destination 

Then : Sync the Application 


### Connect Source Git Repo 

In the UI -> setting Icon -> Repositories -> Connect Repo -> Via HTTPS -> Type Git -> Repo URL

### Where Does Argocd store details ?  username, password, ect ....

Go to `kubectl get secrets -n argocd`

I just created one new Repo above argo cd will create a secret name `repo-xxxxx` data will be store in here 

### Rollback in Argo CD 

Click on `History & Rollback`

ArgoCD will automatically rollback to previous version

### Create Application using Argocd CLI

Step 1 : Login to ArgoCD

`argocd app login localhost:8080`

Step 2 : Check App list

`argocd app list`

Step 3 : Create Application 

```
argocd app create java-app \
--repo 
--path
--dest-namespace
--dest-server
```

Step 4 : Synchronize the Java-App 

`argocd app sync java-app`

### Create ArgoCD Project

**Default Project** : Allow me to deploy any application to any namespace bcs Dest *,*

**Custom Project** : I can add restriction 

Go to Setting Icon in the UI -> Project -> Create Project 

**Source Repo** : Only allow connecting to certain Repo 

**Destination** : 
- Can set up only allow to deploy to specify or current Cluster . 
- Can set up deploy to any Namespace or Specific Namespace

**Cluster Source Allow list**: Can configure to restrict any resource to deploy in the Cluster

**Monitoring** 

**Roles**: Used to control who can perform which action on application to that project

Get project format : `argocd proj get <project-name> -o yaml`





