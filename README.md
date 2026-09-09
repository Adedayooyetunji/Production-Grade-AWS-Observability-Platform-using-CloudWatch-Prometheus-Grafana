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
## CLI

The AWS CLI and Terraform CLI are used to manage and deploy the observability infrastructure.

### AWS CLI

Check your AWS account:

```bash
aws sts get-caller-identity
```

View CloudWatch metrics:

```bash
aws cloudwatch list-metrics
```

### Terraform CLI

Initialize Terraform:

```bash
terraform init
```

Validate the configuration:

```bash
terraform validate
```

Preview changes before deployment:

```bash
terraform plan
```

Review the plan carefully, then deploy:

```bash
terraform apply
```

Terraform will ask for confirmation before making changes. Only approve the deployment if the changes are expected.

For automated environments:

```bash
terraform apply -auto-approve
```

> **Note:** Use `-auto-approve` carefully, especially in production, because it skips manual confirmation.

### Destroy Infrastructure

To remove the infrastructure:

```bash
terraform destroy
```

Review the planned changes before confirming the destruction.

### Prerequisites

Make sure the following are installed and configured:

* AWS CLI
* Terraform
* AWS account and credentials
* Git

Configure AWS credentials with:

```bash
aws configure
```

Verify your configuration:

```bash
aws sts get-caller-identity
```
## Documentation

This project includes documentation to help with setup, deployment, monitoring, and troubleshooting.

### Available Documentation

* **Getting Started** – Project setup and installation.
* **Infrastructure** – AWS resources and Terraform configuration.
* **Monitoring** – CloudWatch, Prometheus, and Grafana setup.
* **Dashboards** – Grafana dashboards and key metrics.
* **Alerting** – Monitoring alerts and notification setup.
* **Troubleshooting** – Common issues and possible solutions.

### Project Structure

```text
docs/
├── setup.md
├── infrastructure.md
├── monitoring.md
├── dashboards.md
├── alerting.md
└── troubleshooting.md
```

Refer to the documentation before deploying changes to the infrastructure.

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
## Scripts

This project includes scripts to make setup, deployment, and monitoring easier.

* `setup.sh` – Sets up the monitoring environment.
* `deploy.sh` – Deploys the infrastructure and services.
* `cleanup.sh` – Removes deployed resources when they are no longer needed.
* `health-check.sh` – Checks the status of the monitoring services.

Run a script with:

```bash
chmod +x scripts/*.sh
./scripts/setup.sh
```
