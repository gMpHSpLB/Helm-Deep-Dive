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

