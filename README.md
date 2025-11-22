# 🛍️ ShopEase – End‐to‐End CI/CD with ArgoCD & Jenkins 

## 📘 Overview

**ShopEase** is a modern, cloud-native e-commerce platform built using **React (Vite)** for the frontend and **Node.js (Express)** for the backend.  
It provides a seamless end-to-end shopping experience for users and a dedicated **admin dashboard** for managing products, orders, and payments through **Stripe**.

The project demonstrates a **hybrid CI/CD pipeline** leveraging **Jenkins**, **ArgoCD**, and **Kustomize** — fully deployed on **AWS EKS**, integrating **DevSecOps**, and **observability** via **Prometheus + Grafana**.

![Demo GIF](https://drive.google.com/uc?export=view&id=1KlZOHzjdOIiuyShQnv3BEbnUR-PFJE0U)


## 🧱 Architecture Summary

| Component      | Technology                         | Purpose                       |
| -------------- | ---------------------------------- | ----------------------------- |
| Frontend       | React (Vite) + Redux               | User & Admin interface        |
| Backend        | Node.js + Express                  | RESTful API server            |
| Database       | Aiven Managed MySQL                | Persistent data store         |
| CI/CD          | Jenkins + ArgoCD + Kustomize       | Automated deployment          |
| Security       | Sealed Secrets + Trivy + SonarQube | Secret management & DevSecOps |
| Monitoring     | Prometheus + Grafana               | Observability & metrics       |
| Cloud Provider | AWS (EKS)                          | Managed Kubernetes cluster    |

---

## 🗄️ Database – Aiven Managed MySQL

- Hosted on **Aiven.io**, ensuring a fully managed MySQL service with automated backups, scaling, and SSL connectivity.
- Separate environments:
  - 🧩 `aiven-mysql-dev` → Development
  - 🚀 `aiven-mysql-prod` → Production
- Aiven provides automated failover and data encryption at rest.

---

## ⚙️ Backend (Node.js + Express)

### Highlights

- Over **25 RESTful API routes** for:
  - Product management (CRUD)
  - Order handling and checkout
  - Secure payments (via **Stripe**)
  - Admin and user management
- Integrates with **Stripe Dashboard** for admin-side analytics and control.
- Uses **JWT authentication**, **Bcrypt** for password hashing, and **Firebase** for media uploads.
- Built with **Docker**, including a fully automated **Jenkinsfile** for CI/CD.

### Backend CI/CD (Jenkins Pipeline)

- Builds and tests backend code.
- Runs **SonarQube** analysis for code quality.
- Executes **Trivy** image scan for vulnerabilities.
- Builds Docker image and pushes to **Docker Hub**.
- ArgoCD **Image Updater** automatically updates manifests in Git with the new image tag.
- **ArgoCD** auto-syncs changes to the **dev environment**, while **prod** updates are **manual (approval-based)**.

---

## 🚀 Backend Deployment

- Managed through **ArgoCD + Kustomize**, enabling clean multi-environment separation.
- **Sealed Secrets** are used to store sensitive information inside Git securely.
- A **self-signed certificate** is generated once and reused across clusters — allowing the same encrypted secrets to be used anywhere that key and cert pair exist.
- Environments:
  - `dev` → Auto-deployed via Jenkins + ArgoCD sync.
  - `prod` → Manual promotion through approval.

---

## 💻 Frontend (React + Vite + Redux)

### Highlights

- Built using **React Vite** for performance and fast development.
- **Redux** handles global state management (auth, admin states).
- **Nginx** is used as a reverse proxy to connect backend and frontend seamlessly.
- The backend is accessed using **Kubernetes Service Names** since both run in the same cluster.

### Frontend CI/CD (Jenkins Pipeline)

- Triggered automatically on push or merge to `main` branch.
- Builds optimized production bundle via Vite.
- SonarQube and Trivy scans (same as backend).
- Builds Docker image and pushes to Docker Hub.
- ArgoCD Image Updater syncs new frontend image tag.
- ArgoCD deploys to `dev` namespace automatically; `prod` remains approval-based.

---

## 🔐 Security & DevSecOps

- **Kubernetes Sealed Secrets** for secure secret management.
- **SonarQube** for continuous code analysis and quality gate enforcement.
- **Trivy** for container image vulnerability scanning.
- **Role-based access** between Jenkins, ArgoCD, and EKS for secure automation.

---

## 📊 Monitoring & Observability

- **Prometheus** collects metrics from workloads.
- **Grafana Dashboards** visualize application health, cluster utilization, and CI/CD metrics.
- Alerts configured for deployment failures and performance degradation.

---

## ☁️ AWS Infrastructure

- **AWS EKS** hosts both frontend and backend workloads.
- **EC2 Server** Jumphost (networking, EKS, IAM roles).
- **Kustomize** manages environment overlays (`dev`, `prod`).
- **ArgoCD** handles declarative, GitOps-based deployments.

---

## 🧩 Project Structure

```
Shopease/
├── Shopease-backend/                         # Node.js Express app (submodule)
├── Shopease-frontend/                        # React Vite app (submodule)
├── Shopease-backend-deployment /             # K8s manifests for backend (submodule)
├── Shopease-frontend-deployment /            # K8s manifests for frontend (submodule)
├── README.md
└── .gitmodules        # Submodule references
```

---

## 🧠 Result

**Jenkins → CI (build, test, scan, push)**  
**ArgoCD + Image Updater → GitOps-based CD**  
**Kustomize → smooth environment promotion (dev → prod)**

**ShopEase** delivers a fully automated, secure, observable, and environment-independent e-commerce deployment pipeline on AWS.

---

## 📎 Author

**Devesh Rathod**  
🚀 Cloud | DevOps | Full Stack Engineer  
📧 [Contact via GitHub](https://github.com/DeveshRathod)
