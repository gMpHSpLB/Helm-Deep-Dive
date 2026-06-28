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

Architecture Diagram:
<img width="1440" height="1840" alt="image" src="./t01_kubernetes_object_map.png" />

The Complete Object Relationship Map:
Namespace: myapp-dev
│
├── ConfigMap: myapp-config
│     └── Data: APP_ENV, APP_LOG_LEVEL, app.ini
│
├── Secret: myapp-secret
│     └── Data: DB_PASSWORD, API_KEY, DATABASE_URL
│
├── Deployment: myapp
│     └── spec.selector: {app: myapp, component: api}
│           └── manages →
│                 ReplicaSet: myapp-<hash>
│                       └── manages →
│                             Pod: myapp-<hash>-<rand> (replica 1)
│                             Pod: myapp-<hash>-<rand> (replica 2)
│                                   └── reads →
│                                         ConfigMap: myapp-config (env vars + volume)
│                                         Secret: myapp-secret (env vars)
│
└── Service: myapp
      └── spec.selector: {app: myapp, component: api}
            └── routes traffic to → Pod (replica 1), Pod (replica 2)

