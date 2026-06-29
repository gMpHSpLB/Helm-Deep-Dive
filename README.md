Project: Production Helm Chart Engineering

Chart: myapp — multi-environment Helm chart with full values schema

What I built:
  - A production-grade Helm chart with 8 templates, complete values schema,
    three environment override files (dev/staging/prod), and a NOTES.txt
    that gives operators the commands they need immediately after install
  - Implemented the checksum annotation pattern to automate pod restarts
    on ConfigMap/Secret changes — eliminating a common manual operations step
  - Implemented HPA/replicas mutual exclusion via conditional rendering —
    preventing the controller-fight anti-pattern that causes scaling oscillation
  - Wrote 12 helm-unittest unit tests covering security context, probe config,
    resource limits, HPA interaction, and secret injection paths
  - Diagnosed and documented four production Helm failure scenarios:
    template rendering errors, silent values merge bugs, FAILED release recovery,
    and values priority override issues

Key skills demonstrated:
  - Go template mechanics: range, with, if/else, include, sha256sum, b64enc
  - Helm values hierarchy and the map-replacement trap
  - Release management: history, rollback, --atomic for CI safety
  - helm-diff for pre-change visibility in production operations
  - Helmfile for declarative multi-environment orchestration
  - Chart unit testing with assertions on rendered templates

The critical Helm mental model:


values.yaml (defaults)
      +
values-dev.yaml (overrides)          ← helm install -f values-dev.yaml
      +
--set key=value (CLI overrides)      ← highest priority, avoid in             
                                                      production
      │
      ▼
helm template engine renders all .yaml files in templates/
      │
      ▼
kubectl apply sends rendered YAML to API server
      │
      ▼
Release stored as Secret in namespace (helm history shows this)
Priority order (highest wins): --set > -f override.yaml > values.yaml

Chart Architecture: What a Production Chart Looks Like:

charts/myapp/
├── Chart.yaml              ← chart metadata: name, version, appVersion, dependencies
├── values.yaml             ← default values (safe for dev, not production)
├── values-dev.yaml         ← dev environment overrides
├── values-staging.yaml     ← staging overrides
├── values-prod.yaml        ← production overrides (strictest settings)
├── .helmignore             ← files to exclude from packaged chart (like .gitignore)
├── NOTES.txt               ← printed to terminal after install/upgrade
├── README.md               ← chart documentation
├── templates/
│   ├── _helpers.tpl        ← named templates (functions reused across templates)
│   ├── namespace.yaml      ← (optional) namespace definition
│   ├── configmap.yaml
│   ├── secret.yaml         ← in prod: ExternalSecret, not Secret
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   ├── hpa.yaml            ← HorizontalPodAutoscaler
│   ├── pdb.yaml            ← PodDisruptionBudget
│   ├── serviceaccount.yaml
│   ├── networkpolicy.yaml
│   └── NOTES.txt
└── tests/
    ├── deployment_test.yaml    ← helm unittest tests
    └── service_test.yaml
