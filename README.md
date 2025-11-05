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
     
     
## STEP 2: Create the **Cattle-system** Namespace
   ```bash
      kubectl create namespace cattle-system
 ```
## STEP 3: Install a TLS Certificate(Recommended via cert-manager)
 - Rancher users HTTPS. You can use **cert-manger** to automate certificates.
 ```bash
 kubectl apply -f https://github.com/cert-manager/cert-manager/releases/latest/download/cert-manager.yaml

   ``` 
- Wait a few minutes for all pods to be ready:
   ```bash
  kubectl get pods -n cert-manager
   ```
## STEP 4: Install Rancher via Helm
 - Replace `your.domain.com` with the domain you'll use for Rancher(it must point to your ingress controller's public IP).
 ```bash
 helm install rancher rancher-latest/rancher \
  --namespace cattle-system \
  --set hostname=your.domain.com \
  --set replicas=1

   ```
## STEP 5: Wait for Rancher to Deploy
   ```bash
      kubectl -n cattle-system rollout status deploy/rancher
 ```
- When it's complete:
   ```bash
  kubectl get pods -n cattle-system
   ```
   - You should see Rancher pods running.
  ---
## STEP 6: Access the Rancher UI
   - Open your browser and go to:
   ```bash
      Https:your.domain.com
 ```
- Set the admin password the first time you log in.
 ---
## Optional: if you Don't Have a Domain
-if you'r testing locally or don't have DNS setup,you can use a`NodePort` or `port-forward` to access Rancher:
   ```bash
      kubectl -n cattle-system port-forward svc/rancher 8443:443
 ```
- Then visit:
    ```bash
      https://localhost:8443
 ```
  
    

