# Continuous Delivery with Jenkins on Kubernetes Engine (GSP051)

## Overview
This lab's goal is to deploy a **scalable Jenkins environment** on **Google Kubernetes Engine (GKE)**.  
It uses Kubernetes for orchestration, Compute Engine persistent disks for Jenkins data, and a Google Cloud HTTP(S) Load Balancer for external access.  

---

## Architecture

<img width="900"  alt="image" src="https://github.com/user-attachments/assets/e550e11b-b8b5-4ad5-9313-5e4f91c8464c" />

### Components

- **Users**
  - Access Jenkins via a public HTTP(S) Load Balancer through the UI Ingress.

- **HTTP(S) Load Balancer**
  - Routes external traffic securely to Jenkins services running inside the Kubernetes cluster.

- **Kubernetes Engine**
  - **Node 1**
    - **Jenkins Master Pod**: Runs the Jenkins controller (web UI, job configuration, plugin management).
    - **Jenkins Executor Pod**: Runs build jobs delegated by the master.
  - **Node 2**
    - **Jenkins Executor Pods**: Scale-out workers to handle build jobs in parallel.
  - **Jenkins UI Service**: Exposes the Jenkins interface to users via Ingress.
  - **Jenkins Discovery Service**: Handles service discovery between the Jenkins master and executor pods.

- **Compute Engine**
  - **Jenkins Data Persistent Disk**: Stores Jenkins configuration, job history, and artifacts to ensure data durability across pod restarts.

---

## Workflow

1. A **user** accesses Jenkins through the **Load Balancer**.
2. The request is forwarded to the **Jenkins UI Service** running in Kubernetes.
3. The **Jenkins Master Pod** coordinates job scheduling and communicates with **Executor Pods**.
4. Build jobs are executed on available **Executor Pods** (across nodes).
5. Persistent job data and configurations are stored on the **Compute Engine Persistent Disk**.

---

## Benefits

- **Scalability**: Executors can scale horizontally across Kubernetes nodes.
- **High Availability**: Kubernetes reschedules pods in case of failure.
- **Durability**: Jenkins data is safely stored on a persistent disk.
- **Flexibility**: Kubernetes services and ingress make it easy to expose Jenkins securely.

---

## Next Steps

- Add CI/CD pipelines that build and deploy containerized applications on GKE.
- Configure Jenkins pipelines to trigger automatically via GitHub or Cloud Build.
- Explore Jenkins plugins for advanced DevOps workflows.
