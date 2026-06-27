Project: Kubernetes Workload Management from First Principles
Environment: minikube on ubuntu (
Components: Namespace, ConfigMap, Secret, Deployment, Service (ClusterIP + NodePort)

What I built:
  - A fully operational Kubernetes environment on local WSL2
  - Deployed a containerised application with proper resource management,
    security context (non-root, read-only filesystem, dropped capabilities),
    and health probes (startup, liveness, readiness)
  - Wired configuration from ConfigMap and credentials from Secret into
    running containers via environment variables and volume mounts
  - Demonstrated zero-downtime rolling updates and automated self-healing
  - Diagnosed and recovered from four production-equivalent failure scenarios:
    ImagePullBackOff, CrashLoopBackOff, OOMKilled, and probe failure

Key skills demonstrated:
  - Kubernetes object model and controller reconciliation loop
  - Resource requests vs limits and their effect on scheduling and OOMKill
  - Health probe design (startup vs liveness vs readiness — not the same thing)
  - Pod security context: non-root, read-only filesystem, dropped capabilities
  - Systematic kubectl-based troubleshooting methodology
  - Rolling update mechanics and rollback procedure
