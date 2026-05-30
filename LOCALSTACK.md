# LocalStack Local Development Override

## Phase 1 — Local Environment (Active)

This file overrides AWS settings in CLAUDE.md for local development.
Read CLAUDE.md first, then apply these overrides.

## Infrastructure Already Running (DO NOT recreate)
- LocalStack 2.3: http://localhost:4566 (secretsmanager, s3, iam)
- Local Docker Registry: http://localhost:5001
- kind cluster: petclinic (3 nodes, k8s v1.35.1)
- MetalLB: IP pool 172.18.255.200-172.18.255.250
- NGINX Ingress: External-IP 172.18.255.200
- mkcert TLS: secret petclinic-tls in namespaces petclinic-dev, monitoring, argocd

## Local Overrides

| Setting | CLAUDE.md (AWS) | Local Override |
|---------|----------------|----------------|
| Region | eu-central-1 | us-east-1 |
| Namespace | petclinic-dev | petclinic-dev (same) |
| Image registry | ECR (AWS) | localhost:5001 |
| Terraform backend | S3 real AWS | S3 LocalStack http://localhost:4566 |
| Secrets | AWS Secrets Manager | LocalStack http://localhost:4566 |
| Ingress | ALB Controller | NGINX Ingress |
| DNS | Route53 + ACM | /etc/hosts + mkcert |
| Kubernetes | EKS | kind cluster |

## Image Naming (Local)
localhost:5001/{service-name}:latest

## Available Secrets in LocalStack
- petclinic/config-server
- petclinic/database
- petclinic/api-gateway
- petclinic/genai
- petclinic/admin

## Docker Images
springcommunity/spring-petclinic-config-server → localhost:5001/config-server:latest
springcommunity/spring-petclinic-discovery-server → localhost:5001/discovery-server:latest
springcommunity/spring-petclinic-api-gateway → localhost:5001/api-gateway:latest
springcommunity/spring-petclinic-customers-service → localhost:5001/customers-service:latest
springcommunity/spring-petclinic-vets-service → localhost:5001/vets-service:latest
springcommunity/spring-petclinic-visits-service → localhost:5001/visits-service:latest
springcommunity/spring-petclinic-genai-service → localhost:5001/genai-service:latest
springcommunity/spring-petclinic-admin-server → localhost:5001/admin-server:latest

## Local URLs
https://petclinic.local         → api-gateway
https://grafana.petclinic.local → grafana
https://zipkin.petclinic.local  → zipkin
https://argocd.petclinic.local  → argocd
