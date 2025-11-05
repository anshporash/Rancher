# _**Rancher**_

## Step-by-step: Install Rancher on Existing Kubernetes
### Prerequisites
 1. **Existing Kubernetes Cluster**
     - Ensure you already have a running Kubernetes cluster.
     - Example: created using `kubeadm`,`k3s`,`minikube`,`EKS`,`AKS` or `GKE`.
 2. **Kubectl access**
     -  You should be able to run `kubectl get nodes` successfully.
 3. **Helm 3 installed**
     ```bash
      curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
     ```
 4. **Ingress Controller**
     - Rancher needs an ingress controller like **NGINX**
     - If not installed:
        ```bash
        kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
        ```

   ---
   
## STEP 1: Add the Rancher Helm Repository
     ```bash
      helm repo add rancher-latest https://releases.rancher.com/server-charts/latest
      helm repo update
     
        ```
     
## STEP 2: Create the **Cattle-system** Namespace
   ```bash
      kubectl create namespace cattle-system

       ```
