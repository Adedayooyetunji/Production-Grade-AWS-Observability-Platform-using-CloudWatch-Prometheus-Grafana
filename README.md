# Production-Grade-AWS-Observability-Platform-using-CloudWatch-Prometheus-Grafana
Production-grade AWS observability platform using Amazon CloudWatch, Prometheus, and Grafana to provide centralized monitoring, metrics collection, visualization, alerting, and infrastructure health insights across AWS workloads.
## Local Development

### Prerequisites

Before getting started, make sure you have the following installed:

* Git
* Docker & Docker Compose
* AWS CLI
* Terraform
* An AWS account

### Clone the Repository

```bash
git clone https://github.com/<your-username>/aws-observability-platform.git
cd aws-observability-platform
```

### Configure AWS CLI

Configure your AWS credentials:

```bash
aws configure
```

Verify your AWS identity:

```bash
aws sts get-caller-identity
```

### Start the Monitoring Stack

Start Prometheus and Grafana using Docker Compose:

```bash
docker compose up -d
```

Verify that the containers are running:

```bash
docker compose ps
```

### Access Grafana and Prometheus

After starting the stack, open:

* Grafana: `http://localhost:3000`
* Prometheus: `http://localhost:9090`

### Deploy AWS Infrastructure

If Terraform is included in the project:

```bash
cd terraform
terraform init
terraform plan
terraform apply
```

### Stop the Environment

To stop the services:

```bash
docker compose down
```

To remove containers, networks, and volumes:

```bash
docker compose down -v
```

### Security

Never commit AWS credentials, API keys, passwords, or other sensitive information to GitHub.

Use AWS IAM roles, environment variables, AWS Secrets Manager, or another secure secrets-management solution instead.

## Infrastructure

This project uses **Terraform** to set up and manage the AWS resources.

* **EC2** – Runs the applications and monitoring tools.
* **CloudWatch** – Collects logs and system metrics.
* **Prometheus** – Collects and monitors application metrics.
* **Grafana** – Displays metrics in easy-to-read dashboards.
* **VPC & IAM** – Provide secure networking and access control.

**Monitoring Flow:**
`AWS → CloudWatch / Prometheus → Grafana → Dashboards & Alerts`
