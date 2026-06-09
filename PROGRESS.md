# PetClinic Capstone - Progress Summary

## Current Status: Prompt 13 IN PROGRESS
- Repo: https://github.com/gregodprogrammer/petclinic-platform-final
- Working directory: ~/petclinic-platform-final
- kind cluster: petclinic (3 nodes, k8s v1.35.1)
- namespace: petclinic-dev

## Completed Prompts
- Prompt 1-11: All complete ✅
- Prompt 12: Prometheus + Grafana + AlertManager ✅
  - Grafana: http://localhost:3300 (admin / petclinic-grafana)
  - Grafana service: svc/grafana -n monitoring
  - Port forward: kubectl port-forward -n monitoring svc/grafana 3300:80 --address=0.0.0.0 &

## Prompt 13 Status: IN PROGRESS
- metrics-server: CrashLoopBackOff (port 10250 conflict with hostNetwork)
- Last error: bind: address already in use on port 10250
- Last attempted fix: port 4443 deployment (not yet run)
- HPA manifests: NOT YET CREATED

## Current Issues To Fix On Resume
1. petclinic-dev pods showing Unknown - need force delete to recover
2. metrics-server CrashLoopBackOff - fix with port 4443
3. Then create HPA for all 6 services

## Startup Commands (run every session)
pkill -f "port-forward" 2>/dev/null
sleep 2
kubectl port-forward -n ingress-nginx svc/ingress-nginx-controller 8443:443 --address=0.0.0.0 &
kubectl port-forward -n ingress-nginx svc/ingress-nginx-controller 8080:80 --address=0.0.0.0 &
kubectl port-forward -n monitoring svc/grafana 3300:80 --address=0.0.0.0 &

## Infrastructure
- WSL2 IP: 172.24.140.135
- MetalLB pool: 172.18.255.200-250
- NGINX Ingress External-IP: 172.18.255.200
- TLS certs: ~/petclinic-capstone/k8s/tls/
- LocalStack: port 4566
- kind cluster name: petclinic

## Windows hosts file entries
172.24.140.135 petclinic.local
172.24.140.135 grafana.petclinic.local
172.24.140.135 zipkin.petclinic.local
172.24.140.135 argocd.petclinic.local
172.24.140.135 prometheus.petclinic.local
172.24.140.135 alertmanager.petclinic.local

## Screenshot Naming Convention
p{prompt}-{sequence}-{description}.png
Last screenshot: p12-16-progress-md-updated.png
Next screenshot: p13-01-...
