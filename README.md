# Microservices Deployment Project with Kubernetes and Helm

## Overview
This project showcases a comprehensive deployment pipeline for Google's Online Boutique, a cloud-native microservices demo application. The deployment architecture is implemented across three progressive phases, leveraging Kubernetes (Linode LKE), Helm, and Helmfile.

> 🛍️ **Demo Application**: This project deploys [Google's Online Boutique](https://github.com/GoogleCloudPlatform/microservices-demo), a cloud-native microservices application consisting of 11 services that together form a fully-functional e-commerce platform.


![Diagram](https://github.com/Princeton45microservices-helm-deployment1/blob/main/images/diagram.jpg)

## Phase 1: Kubernetes Deployment with Production & Security Best Practices

### What I Built
- Created complete Kubernetes manifests for an online shop application
- Deployed microservices on Linode Kubernetes Engine (LKE)
- Implemented Redis for caching and session management
- Applied production-grade security configurations

[Kubernetes Dashboard Screenshot]

### Technologies Used
- Kubernetes
- Redis
- Linux
- Linode LKE

## Phase 2: Helm Chart Implementation

### What I Built
- Developed a unified Helm chart for all microservices
- Implemented reusable configurations for Deployments and Services
- Created standardized templates for consistent deployments

[Helm Architecture Diagram]

### Technologies Used
- Kubernetes
- Helm

## Phase 3: Helmfile Deployment

### What I Built
- Streamlined deployment process using Helmfile
- Automated multi-service deployments
- Implemented environment-specific configurations

[Deployment Flow Diagram]

### Technologies Used
- Kubernetes
- Helm
- Helmfile

