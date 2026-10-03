# AWS EKS Kubernetes Autoscaling Project

## Project Overview

This project demonstrates the deployment of a containerized WordPress application on Amazon EKS using Kubernetes and Helm. The application is exposed publicly through an AWS Load Balancer and configured to automatically scale based on CPU utilization using a Horizontal Pod Autoscaler (HPA).

Load testing was performed with Siege to simulate increased traffic and verify Kubernetes autoscaling.

## Technologies Used

- AWS EKS
- Kubernetes
- Helm
- AWS Elastic Load Balancer
- Horizontal Pod Autoscaler (HPA)
- Siege
- Linux / WSL
- AWS CLI
- eksctl
- kubectl

## Architecture

```text
                Internet
                   |
                   v
          AWS Load Balancer
                   |
                   v
          Kubernetes Service
                   |
                   v
        WordPress Deployment
                   |
            +------+------+
            |             |
           Pod           Pod
            |             |
            +------+------+
                   |
                   v
         Horizontal Pod Autoscaler
                   |
            CPU Target: 50%
                   |
             1 - 5 Pods

## Deployment
The EKS cluster was created with three managed worker nodes using eksctl.
The WordPress application was packaged as a Helm chart and deployed to the EKS cluster.
"helm install my-microservice ./my-microservice"
The application was exposed using a Kubernetes LoadBalancer Service, which provisioned an AWS Elastic Load Balancer for public access.

## Horizontal Pod Autoscaling
A Kubernetes Horizontal Pod Autoscaler was configured with:
- Minimum replicas: 1
- Maximum replicas: 5
- Target CPU utilization: 50%
"kubectl apply -f hpa.yaml"

## Load Testing
Siege was used to simulate 100 concurrent users against the application.
"siege -c 100 -t 5m http://<SERVICE-IP>"

During the test, CPU utilization increased from approximately 3% to over 100%.
The HPA detected the increased CPU usage and automatically scaled the WordPress deployment from 1 Pod to 5 Pods

## Troubleshooting
During load testing, the original Siege test generated DNS resolution errors when accessing the Kubernetes Service by hostname.
I isolated the issue by:
1. Verifying the AWS Load Balancer could reach WordPress.
2. Testing the Kubernetes Service from another Pod.
3. Verifying Kubernetes DNS resolution.
4. Confirming the Siege container could resolve the Service hostname.
5. Testing Siege directly against the Kubernetes Service IP.
The Service IP test succeeded with 100% availability, allowing the load test and HPA validation to continue.

## Results
The project successfully demonstrated:
- Deploying workloads to Amazon EKS
- Packaging Kubernetes resources with Helm
- Exposing applications through an AWS Load Balancer
- Monitoring CPU utilization with Kubernetes metrics
- Automatically scaling workloads with HPA
- Performing load testing with Siege
- Troubleshooting Kubernetes networking and DNS issues




