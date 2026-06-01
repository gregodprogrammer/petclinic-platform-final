
## Phase 1 Progress Summary (as of 2026-05-31)

### Completed Work
- Repo: https://github.com/gregodprogrammer/petclinic-platform-final
- All infrastructure running locally

### Infrastructure Status
- LocalStack 2.3: running on port 4566 (secretsmanager, s3, iam)
- Local Docker registry: running on port 5001 (all 8 images pushed)
- kind cluster: petclinic (3 nodes, k8s v1.35.1)
- MetalLB: IP pool 172.18.255.200-172.18.255.250
- NGINX Ingress: External-IP 172.18.255.200
- Port forwards: 8443→443, 8080→80 (must restart after reboot)
- WSL2 IP: 172.24.140.135

### Kubernetes Status (namespace: petclinic-dev)
- config-server: Running ✅
- discovery-server: Running ✅
- api-gateway: Running ✅
- customers-service: Running ✅ (MySQL backed)
- vets-service: Running ✅ (MySQL backed)
- visits-service: Running ✅ (MySQL backed)
- genai-service: Running ✅
- admin-server: Running ✅
- mysql: Running ✅
- zipkin: Running ✅

### APIs Verified
- GET /api/vet/vets → 6 vets ✅
- GET /api/customer/owners → 10 owners ✅
- GET /api/visit → 200 OK ✅
- Zipkin traces: 4 services sending spans ✅

### Windows Access
- URL: https://petclinic.local:8443
- Windows hosts file updated: 172.24.140.135 for all domains
- Port forwards must be restarted after WSL2 restart:
  kubectl port-forward -n ingress-nginx svc/ingress-nginx-controller 8443:443 --address=0.0.0.0 &
  kubectl port-forward -n ingress-nginx svc/ingress-nginx-control
### Next Prompts Remaining
- Prompt 12: Prometheus + Grafana + AlertManager
- Prompt 13: metrics-server + HPA + Load Test
- Prompt 14: ArgoCD + App-of-Apps + Badge
- Prompt 15: External Secrets Operator
- Prompt 16: GitHub Actions CI
- Prompt 17: Full End-to-End Verification
- Prompt 18+: AWS Phase

### Key Files
- Helm chart: helm/petclinic-service/
- Per-service values: helm-values/
- MySQL manifest: k8s/base/mysql.yaml
- Ingress: k8s/base/ingress.yaml
- TLS certs: ~/petclinic-capstone/k8s/tls/

### Challenges Encountered (13 total)
See docs/CHALLENGES.md for full list
Key ones:
- LocalStack ECR is Pro-only → replaced with local Docker registry
- kind nodes cant reach localhost:5001 → imagePullPolicy: IfNotPresent
- Eureka hostname vs IP → EUREKA_INSTANCE_PREFER_IP_ADDRESS=true
- Zipkin at localhost:9411 → deployed zipkin to petclinic-dev namespace
- Windows cant reach WSL2 MetalLB IP → kubectl port-forward on 0.0.0.0
