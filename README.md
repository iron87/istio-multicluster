# Istio Multicluster Mesh

This repository provides a comprehensive guide to deploying [Istio](https://istio.io/) in a multicluster Kubernetes environment. It includes instructions for provisioning clusters, installing Istio, deploying a demo app, and configuring advanced service mesh features.

## Architecture Overview

The setup includes:
- Multiple Kubernetes clusters
- A shared root CA for identity federation
- Istio installed in each cluster with east-west gateway configuration

## Repository Structure

| File | Description |
|------|-------------|
| `00-gke-cluster-provisioning.md` | Instructions to provision GKE clusters |
| `01-istio-installation.md` | Guide to install Istio across multiple clusters |
| `02-demo-app.md` | Deployment of a sample application |
| `03-locality-awarel-lb.md` | Configure locality-aware load balancing |
| `04-external-service-traffic.md` | Manage traffic to external services |
| `05-zero-trust-security.md` | Implement zero-trust policies with Istio |

## Quick Start

1. **Provision Kubernetes Clusters**  
   Follow `00-gke-cluster-provisioning.md` to create the necessary clusters.

2. **Install Istio**  
   Use `01-istio-installation.md` to install Istio with multicluster support.

3. **Deploy a Sample App**  
   Steps in `02-demo-app.md`.

4. **Configure Mesh Policies**  
   Explore locality-based routing, external service access, and security using the respective guides.

## Requirements

- Multiple Kubernetes clusters (e.g., GKE)
- `kubectl`, `istioctl`, and `helm` CLI tools
- Basic knowledge of Istio and Kubernetes


---

For more information about Istio multicluster deployments, visit [istio.io](https://istio.io/latest/docs/setup/install/multicluster/).
