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

## Prompt 13 — metrics-server + HPA Autoscaling — COMPLETE

### What was done
- Diagnosed root cause of ALL cluster instability: WSL2 fs.inotify.max_user_instances=128 (too low)
- Fixed inotify limits on WSL2 host and kind node (8192 instances, 524288 watches)
- Persisted fix to /etc/sysctl.d/99-petclinic.conf
- Fixed kube-proxy CrashLoopBackOff (was failing with "too many open files")
- metrics-server recovered once kube-proxy restored iptables routing
- Created k8s/hpa/hpa-all-services.yaml with HPA for all 8 services
- Verified kubectl top nodes and kubectl top pods working
- HPA auto-scaled without load test: api-gateway/customers/vets/visits all hit maxReplicas
- Git committed and pushed: 7e4c65a

### Screenshots
- p13-05: kubectl top nodes + kubectl top pods showing real metrics
- p13-06: kubectl get hpa showing real CPU%/memory% targets with replicas scaled
- p13-07: All pods running with multiple replicas + HPA status

### Key facts
- inotify fix: fs.inotify.max_user_instances=8192, max_user_watches=524288
- metrics-server image: registry.k8s.io/metrics-server/metrics-server:v0.7.2
- metrics-server port: 4443, hostNetwork: true
- HPA file: k8s/hpa/hpa-all-services.yaml
- vets-service memory running at 121%/75% threshold — needs limit increase (documented)
- genai-service: 1/1 Running, init containers wait for config+discovery servers
- Large file writes: use python3 write_text not heredoc (WSL2 terminal paste corruption)

## Next: Prompt 14 — ArgoCD + app-of-apps + badge

## Session End — 9 June 2026

### Cluster Status
- kube-proxy: was fixed, may have crashed again after reboot
- inotify limits: reset on reboot — must reapply every WSL2 restart
- metrics-server: was working, 8 restarts
- HPA: all 8 HPAs created, auto-scaling verified
- App: 503/502 on https://petclinic.local:8443 — pods warming up

### On Resume — Run These First
1. sudo sysctl -p /etc/sysctl.d/99-petclinic.conf
2. docker exec petclinic-control-plane sysctl -w fs.inotify.max_user_instances=8192
3. kubectl get pods -n kube-system | grep kube-proxy
4. kubectl scale deployment api-gateway customers-service vets-service visits-service admin-server genai-service -n petclinic-dev --replicas=1
5. kubectl port-forward -n ingress-nginx svc/ingress-nginx-controller 8443:443 --address=0.0.0.0 &

### DMI Submission Documents (already generated)
- DMI_Cohort2_Greg_COPYREADY.docx — main submission document
- DMI_Posts_Greg.docx — all 4 LinkedIn and blog posts
- Both in /mnt/user-data/outputs/ — download before closing this chat

### Screenshots Still Needed
- B2-S3: docker compose ps
- B2-S4a: localhost:8080 homepage
- B2-S4b: localhost:8761 Eureka
- B2-S4c: localhost:9090 Spring Boot Admin
- B2-S4f: localhost:3030 Grafana
- B2-S5a: Find Owners list
- B2-S5b: Owner detail + pets
- B2-S5c: Add Visit submitted
- B2-S5d: Veterinarians list
- B2-S7: docker compose down output

### Next Prompt on Resume
- Fix kube-proxy if crashed
- Scale down all deployments to 1 replica
- Get petclinic.local:8443 working
- Take the 9 missing screenshots
- Submit DMI document

## Next Capstone Prompt: Prompt 14 — ArgoCD
