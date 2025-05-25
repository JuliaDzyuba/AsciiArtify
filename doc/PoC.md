# Proof of Concept: ASCII-ARTIFY

## Goal
Develop a software product that converts images into ASCII-Art using Machine Learning algorithms. This approach enhances conversion quality by considering image-specific details for more precise rendering.


## Technical Details
### **k3d**
*K3d* is a lightweight tool for creating local Kubernetes clusters, enabling fast deployment of test environments. Advantages:
- Simple setup and rapid deployment.
- Efficient resource utilization compared to full-fledged Kubernetes.
- Easy integration with CI/CD workflows.

### **GitHub**
*GitHub* serves as the version control system for storing code and collaborating on the project. Advantages:
- Reliability and security of repositories.
- Convenient collaboration and version management.
- CI/CD integration for automation of development processes.

### **ArgoCD**
*ArgoCD* is a declarative GitOps system for Kubernetes that provides:
- Automated application deployment on the cluster.
- Visualization of deployment status via a graphical interface.
- Control and security for deployment processes.

## Expected Outcomes
- Set up the k3d cluster for local development.
- Implement the GitOps approach using ArgoCD.
- Ensure team access to the ArgoCD graphical interface for deployment management.

## Conclusions
This PoC will validate the feasibility of automating deployment through a Kubernetes cluster based on k3d. The next stage involves optimizing and integrating a full CI/CD pipeline.


### **Demo**
Demo link [![Demo link](https://asciinema.org/a/baDRqKgEsHDIwiXXzASPuzshc)](https://asciinema.org/a/baDRqKgEsHDIwiXXzASPuzshc)
```
# create new local cluster
$ k3d cluster create argo

$ kubectl cluster-info
Kubernetes control plane is running at https://0.0.0.0:42115
CoreDNS is running at https://0.0.0.0:42115/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
Metrics-server is running at https://0.0.0.0:42115/api/v1/namespaces/kube-system/services/https:metrics-server:https/proxy

$ kubectl version
$ kubectl get all -A

# create new namespace argocd
$ kubectl create namespace argocd
namespace/argocd created

$ kubectl get ns
NAME              STATUS   AGE
argocd            Active   24s
default           Active   2m36s
kube-node-lease   Active   2m36s
kube-public       Active   2m36s
kube-system       Active   2m36s"

# install ArgoCD
$ kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# check containers status
$ kubectl get all -n argocd

# set up port forwarding from local port 8080 to remote 443
$ kubectl port-forward svc/argocd-server -n argocd 8080:443&

# Obtaining a password to access the ArgoCD GUI interface
$ kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo 

# open ArgoCD GUI https://127.0.0.1:8080/
![ArgoCD Admin Panel](argo_admin.png)

```
