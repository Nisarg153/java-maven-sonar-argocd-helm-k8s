# Jenkins Pipeline for Java based application using Maven, SonarQube, Argo CD, Helm and Kubernetes

## About This Project

This project implements a complete CI/CD pipeline for a Java Spring Boot application. The pipeline automates code building, testing, static code analysis, containerization, and deployment to a Kubernetes cluster using GitOps principles.

I built this to deepen my hands-on understanding of how Jenkins, SonarQube, Docker, ArgoCD, and Kubernetes work together in a real deployment workflow. The Jenkinsfile was written and customized by me to fit my infrastructure setup.

> **Inspired by:** [Abhishek Veeramalla's DevOps pipeline tutorial](https://www.youtube.com/watch?v=JGQI5pkK82w) — I followed the core concepts and then adapted the pipeline configuration, infrastructure choices, and deployment approach to my own environment.

## Architecture

![Screenshot 2023-03-28 at 9 38 09 PM](https://user-images.githubusercontent.com/43399466/228301952-abc02ca2-9942-4a67-8293-f76647b6f9d8.png)

## Pipeline Stages

| Stage | Tool | What It Does |
|-------|------|--------------|
| **Checkout** | Git | Pulls latest source code from GitHub |
| **Build** | Maven | Compiles the Java Spring Boot application |
| **Test** | JUnit | Runs unit tests |
| **Static Analysis** | SonarQube | Checks code quality, bugs, vulnerabilities, and code smells |
| **Build & Push Image** | Docker | Builds container image and pushes to DockerHub |
| **Update Manifests** | Shell/Git | Updates the Kubernetes deployment YAML with the new image tag |
| **Deploy** | ArgoCD | Detects manifest changes and syncs the deployment to the K8s cluster |

---
## Infrastructure Setup

| Component | Where It Runs |
|-----------|---------------|
| Jenkins Server | AWS EC2 (Ubuntu) |
| SonarQube | Docker container on EC2 |
| Docker Registry | DockerHub |
| Kubernetes Cluster | Minikube (local) |
| ArgoCD | Deployed on Minikube via ArgoCD Operator |

---

## Tech Stack

- **Language:** Java (Spring Boot)
- **Build Tool:** Maven
- **CI Server:** Jenkins (with custom Jenkinsfile)
- **Code Quality:** SonarQube
- **Containerization:** Docker
- **Container Registry:** DockerHub
- **GitOps:** ArgoCD (Operator-based install)
- **Orchestration:** Kubernetes (Minikube)
- **Manifests:** Kubernetes YAML (deployment + service)
- **Monitoring:** Prometheus & Grafana (configuration included)

---

## Project Structure

```
├── spring-boot-app/              # Java Spring Boot application source code
│   ├── src/                      # Application source and tests
│   ├── pom.xml                   # Maven build configuration
│   ├── Dockerfile                # Multi-stage Docker build
│   └── JenkinsFile               # CI/CD pipeline definition (customized)
├── spring-boot-app-manifests/    # Kubernetes deployment & service YAML
├── Argo CD/                      # ArgoCD application configuration
├── monitoring/                   # Prometheus & Grafana setup files
└── README.md

## What I Built & Customized

- **Jenkinsfile:** Wrote and modified the pipeline stages to work with my EC2 + Minikube setup, including SonarQube integration, Docker build/push, and automated manifest updates via Git.
- **Infrastructure:** Set up Jenkins on AWS EC2, ran SonarQube as a Docker container, and configured ArgoCD on Minikube using the ArgoCD Operator.
- **GitOps Workflow:** Configured ArgoCD to watch the `spring-boot-app-manifests/` directory and automatically sync deployments when the image tag changes.
- **End-to-End Verification:** Successfully deployed the Spring Boot application to Kubernetes and verified it was running and accessible.

---

Here are the step-by-step details to set up an end-to-end Jenkins pipeline for a Java application using SonarQube, Argo CD, Helm, and Kubernetes:

Prerequisites:

   -  Java application code hosted on a Git repository
   -   Jenkins server
   -  Kubernetes cluster
   -  Helm package manager
   -  Argo CD

Steps:

    1. Install the necessary Jenkins plugins:
       1.1 Git plugin
       1.2 Maven Integration plugin
       1.3 Pipeline plugin
       1.4 Kubernetes Continuous Deploy plugin

    2. Create a new Jenkins pipeline:
       2.1 In Jenkins, create a new pipeline job and configure it with the Git repository URL for the Java application.
       2.2 Add a Jenkinsfile to the Git repository to define the pipeline stages.

    3. Define the pipeline stages:
        Stage 1: Checkout the source code from Git.
        Stage 2: Build the Java application using Maven.
        Stage 3: Run unit tests using JUnit and Mockito.
        Stage 4: Run SonarQube analysis to check the code quality.
        Stage 5: Package the application into a JAR file.
        Stage 6: Deploy the application to a test environment using Helm.
        Stage 7: Run user acceptance tests on the deployed application.
        Stage 8: Promote the application to a production environment using Argo CD.

    4. Configure Jenkins pipeline stages:
        Stage 1: Use the Git plugin to check out the source code from the Git repository.
        Stage 2: Use the Maven Integration plugin to build the Java application.
        Stage 3: Use the JUnit and Mockito plugins to run unit tests.
        Stage 4: Use the SonarQube plugin to analyze the code quality of the Java application.
        Stage 5: Use the Maven Integration plugin to package the application into a JAR file.
        Stage 6: Use the Kubernetes Continuous Deploy plugin to deploy the application to a test environment using Helm.
        Stage 7: Use a testing framework like Selenium to run user acceptance tests on the deployed application.
        Stage 8: Use Argo CD to promote the application to a production environment.

    5. Set up Argo CD:
        Install Argo CD on the Kubernetes cluster.
        Set up a Git repository for Argo CD to track the changes in the Helm charts and Kubernetes manifests.
        Create a Helm chart for the Java application that includes the Kubernetes manifests and Helm values.
        Add the Helm chart to the Git repository that Argo CD is tracking.

    6. Configure Jenkins pipeline to integrate with Argo CD:
       6.1 Add the Argo CD API token to Jenkins credentials.
       6.2 Update the Jenkins pipeline to include the Argo CD deployment stage.

    7. Run the Jenkins pipeline:
       7.1 Trigger the Jenkins pipeline to start the CI/CD process for the Java application.
       7.2 Monitor the pipeline stages and fix any issues that arise.

This end-to-end Jenkins pipeline will automate the entire CI/CD process for a Java application, from code checkout to production deployment, using popular tools like SonarQube, Argo CD, Helm, and Kubernetes.

## Key Learnings

- **ArgoCD setup** was the most challenging part — understanding how the ArgoCD Operator works, configuring it to watch a specific path in the repo, and troubleshooting sync issues.
- **GitOps model** where Jenkins handles CI (build, test, push) and ArgoCD handles CD (deploy) provides a clean separation of concerns.
- **SonarQube integration** in the pipeline helped catch code quality issues early before the Docker image is built.
- Learned how **Jenkins updates Git manifests** with new image tags, which then triggers ArgoCD's sync — closing the full CI/CD loop.

---

