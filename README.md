# Production-Grade AWS Observability Platform
### CloudWatch + Prometheus + Grafana

A Git-driven observability stack for AWS workloads: metrics, logs, alerting, and SLO dashboards, deployed entirely as code.

## Overview

| Layer | Tool | Responsibility |
|---|---|---|
| AWS-managed services | Amazon CloudWatch | ALB, RDS, Lambda, SQS, ECS/EKS control plane metrics; service logs; native alarms |
| Application and Kubernetes | Prometheus (kube-prometheus-stack or Amazon Managed Prometheus) | App `/metrics`, node, kube-state-metrics; Alertmanager routing |
| Visualization | Grafana (Amazon Managed Grafana or self-hosted) | Unified dashboards across CloudWatch, Prometheus, and logs |

## Architecture

```
 AWS services ──► CloudWatch Metrics/Logs ──┐
                       │ alarms             │
                       ▼                    ▼
                  SNS ─► PagerDuty/Slack   Grafana ◄── Prometheus / AMP
                                              ▲            ▲
                                        Loki / CW Logs     │ remote_write
                                                     EKS: exporters,
                                                     kube-state-metrics,
                                                     app /metrics
                                                     Alertmanager ─► PagerDuty/Slack
```

## Features

- **Infrastructure as code:** Terraform for VPC, EKS, AMP/AMG, IRSA roles, and CloudWatch alarms; Helm/Argo CD for in-cluster components.
- **High availability:** multi-replica Prometheus, remote write to AMP or Thanos, persistent volumes, clustered Alertmanager.
- **Security:** IRSA (no static keys), private endpoints, SSO for Grafana, read-only least-privilege CloudWatch access.
- **SLO-driven alerting:** multi-window burn-rate alerts on availability and latency SLIs, each with a runbook link.
- **Dashboards as code:** provisioned from JSON or Grafonnet, reviewed via pull requests.
- **Cost control:** log retention policies, custom metric cardinality limits, dropped high-cardinality labels.

## Repository Layout

```
terraform/      # network, eks, amp, amg, cloudwatch alarms
helm/           # kube-prometheus-stack values, exporters
dashboards/     # Grafana JSON / Grafonnet
alerts/         # Prometheus rules + CloudWatch alarm definitions
docs/           # architecture diagram, runbooks, SLOs
```

## Prerequisites

- AWS account with permissions to create VPC, EKS, IAM, AMP, and AMG resources
- Terraform >= 1.6, kubectl, Helm 3, AWS CLI v2

## Quick Start

```bash
# 1. Provision infrastructure
cd terraform
terraform init
terraform apply -var-file=env/prod.tfvars

# 2. Connect to the cluster
aws eks update-kubeconfig --name <cluster-name> --region <region>

# 3. Install the Prometheus stack
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm upgrade --install kps prometheus-community/kube-prometheus-stack \
  -n monitoring --create-namespace -f helm/kube-prometheus-stack/values.yaml

# 4. Apply alert rules
kubectl apply -f alerts/prometheus/
```

## Alerting Strategy

1. **Page** only on symptoms tied to SLOs (error-budget burn), not raw CPU or memory.
2. **Ticket** for slow-burn and capacity issues.
3. Every alert carries `severity`, `service`, and `runbook_url` labels/annotations.
4. Alertmanager groups and inhibits related alerts to reduce noise.

Example burn-rate rule (99.9% availability SLO):

```yaml
- alert: HighErrorBudgetBurn
  expr: |
    (sum(rate(http_requests_total{code=~"5.."}[5m])) / sum(rate(http_requests_total[5m]))) > (14.4 * 0.001)
    and
    (sum(rate(http_requests_total{code=~"5.."}[1h])) / sum(rate(http_requests_total[1h]))) > (14.4 * 0.001)
  for: 2m
  labels: { severity: page }
  annotations:
    summary: "Fast error-budget burn"
    runbook_url: "https://github.com/<org>/<repo>/blob/main/docs/runbooks/error-budget.md"
```

## Validation

- Load and chaos tests confirm alerts fire and route correctly.
- Quarterly game-day exercise using the runbooks in `docs/runbooks/`.

## Roadmap

- [ ] Distributed tracing (OpenTelemetry + X-Ray or Tempo)
- [ ] Log-to-metric correlation dashboards
- [ ] Automated cost reports for observability spend

## License

MIT
