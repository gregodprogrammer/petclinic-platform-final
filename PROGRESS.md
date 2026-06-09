# PetClinic Capstone - Progress Summary

## Current Status: Prompt 13 IN PROGRESS
- Repo: https://github.com/gregodprogrammer/petclinic-platform-final
- Working directory: ~/petclinic-platform-final
- kind cluster: petclinic (3 nodes, k8s v1.35.1)
- namespace: petclinic-dev

## Completed Prompts
- Prompt 1-12: All complete ✅
- Prompt 12: Prometheus + Grafana + AlertManager ✅
  - Grafana: http://localhost:3300 (admin / petclinic-grafana)
  - Grafana service: svc/grafana -n monitoring

## Prompt 13 Status: IN PROGRESS
- metrics-server: CrashLoopBackOff - port 4443 fix NOT YET RUN
- HPA manifests: NOT YET CREATED
- CoreDNS: was broken, restarted and fixed
- discovery-server: stuck in Init due to stale DNS, force deleted awaiting restart

## Current Pod Status (petclinic-dev)
- admin-server: 1/1 Running ✅
- api-gateway: 1/1 Running ✅
- config-server: 1/1 Running ✅
- customers-service: 1/1 Running ✅
- discovery-server: Init:0/1 (being fixed) ⚠️
- genai-service: 0/1 Running ⚠️
- mysql: 1/1 Running ✅
- vets-service: 1/1 Running ✅
- visits-service: 1/1 Running ✅
- zipkin: 1/1 Running ✅

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
Last screenshot: p13-02
Next screenshot: p13-03
