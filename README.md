# ☕ Coffee Shop Deployment on GKE with Secure CI/CD Pipeline

## 📖 Overview

This project demonstrates the deployment of the `coffee-shop` application to a **Google Kubernetes Engine (GKE)** cluster. The deployment leverages a secure **CI/CD pipeline** implemented with **GitHub Actions**, integrating **Workload Identity Federation** for authentication with **Google Cloud Platform (GCP)**.

---

## 🏗️ Architecture

### **Infrastructure**:
- 🛡️ **GKE Cluster**:
  - Deployed in a **private subnet** for enhanced security.
  - Kubernetes manifests designed with production-ready configurations.
- 🔑 **Secrets Management**:
  - Managed via **GCP Secret Manager** and **External Secrets Operator**.

### **CI/CD Pipeline**:
- 🛠️ **Automation**:
  - CI/CD pipeline implemented with **GitHub Actions**.
- 🚀 **Container Registry**:
  - Docker images stored in **Google Container Registry (GCR)**.
- 🔒 **Authentication**:
  - Secure authentication via **Workload Identity Federation**.

---

## 📂 Deployment Files

### 📝 Kubernetes Manifests

1. **`k8s-manifests.yaml`**:
   - Defines the `coffee-shop` **Deployment** and **Service**.
   - Implements security best practices:
     - 👤 Non-root user (`runAsUser: 1000`) and **read-only root filesystem**.
     - 🚫 Disabled privilege escalation.
     - 📊 Resource requests and limits to manage workloads efficiently.

2. **`external-secrets.yaml`**:
   - Integrates **GCP Secret Manager** with Kubernetes via **External Secrets**.
   - Manages sensitive environment variables, such as:
     - Database credentials.
     - Application secrets.

---

### ⚙️ CI/CD Pipeline

- **`deploy.yaml`**:
  - Automated pipeline triggered on commits to the `main` branch.
  - Key steps:
    1. 🔑 Authenticate with **GCP** using **Workload Identity Federation**.
    2. 🛠️ Build and push Docker images with commit-based tags.
    3. 📝 Dynamically update Kubernetes manifests with the latest image tag.
    4. 🚀 Deploy and monitor the `coffee-shop` application.

---

## 🚀 Deployment Steps

1. **Clone the Repository**:

   ```bash
   git clone https://github.com/shonevarkey/gcp-github-actions-gke-pipeline.git
   cd gcp-github-actions-gke-pipeline
   ```
   
3. **Configure Secrets in GCP**:

- Store database credentials and application secrets in GCP Secret Manager.
- Link these secrets to External Secrets using external-secrets.yaml.

3. **Deploy via CI/CD**:

Push changes to the main branch to trigger the pipeline:

```bash
git push origin main
```

4. **Monitor Deployment**:

Use kubectl to monitor the rollout and application status:

```bash
kubectl get pods
kubectl rollout status deployment/coffee-shop
```
---

 ### 🔄 Rollback Procedure
 
In case of a failed deployment, the CI/CD pipeline automatically reverts to the previous image tag:

```bash
kubectl rollout undo deployment coffee-shop 
```

