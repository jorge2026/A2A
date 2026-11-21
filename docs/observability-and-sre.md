# Observability and SRE

## Golden signals

Measure the following for each critical workload:

- **Latency:** request duration, p50/p95/p99.
- **Traffic:** requests per second and concurrent requests.
- **Errors:** HTTP 4xx/5xx, application failures, restarts.
- **Saturation:** CPU, memory, disk, network, and pending Pods.

## Kubernetes signals

| Signal | Example source | Why it matters |
|---|---|---|
| Pod restarts | kube-state-metrics | Crash loops and instability |
| Desired vs available replicas | kube-state-metrics | Deployment health |
| CPU/memory usage | Metrics Server / Prometheus | Capacity and right-sizing |
| Node pressure | kubelet / node exporter | Cluster capacity |
| Ingress latency/errors | Ingress controller metrics | User experience |
| API server health | Control-plane metrics | Cluster reliability |

## Recommended stack

- Prometheus for metrics collection.
- Grafana for dashboards.
- Alertmanager for alert routing.
- Fluent Bit or OpenTelemetry Collector for log pipelines.
- OpenTelemetry instrumentation for traces.

These tools are architecture recommendations; this repository does not install them.

## SLO process

1. Identify user-facing critical journeys.
2. Define SLIs for availability, latency, and correctness.
3. Agree on SLO targets with service owners.
4. Establish error budgets and release policies.
5. Review incidents and adjust capacity or architecture.

Do not claim an availability percentage until it is measured over an agreed period.

## Incident runbook

1. Check deployment rollout status: `kubectl rollout status deployment/demo-web -n platform-demo`.
2. Inspect Pods: `kubectl get pods -n platform-demo -o wide`.
3. Review events: `kubectl get events -n platform-demo --sort-by=.lastTimestamp`.
4. Inspect logs: `kubectl logs deployment/demo-web -n platform-demo --all-containers=true`.
5. Check HPA: `kubectl get hpa -n platform-demo`.
6. Roll back a faulty rollout if required: `kubectl rollout undo deployment/demo-web -n platform-demo`.
