# Kubernetes Ansible Configuration

Ansible automation repository for configuring AWS EKS worker nodes and generating monitoring configurations.

## Technologies

- Ansible
- AWS EC2 Dynamic Inventory
- Amazon EKS
- Ubuntu Linux
- Node Exporter
- Prometheus
- Alertmanager

## Repository Structure

```text
k8s-ansible-config/
├── ansible.cfg
├── requirements.yml
├── inventory/
│   └── aws_ec2.yml
├── playbooks/
│   └── site.yml
├── group_vars/
│   └── all.yml
└── roles/
    ├── node-setup/
    ├── node-exporter/
    ├── prometheus-config/
    └── alertmanager-config/
```

## Ansible Roles

| Role | Responsibility |
|---|---|
| node-setup | Install system packages and configure containerd |
| node-exporter | Install and configure Node Exporter |
| prometheus-config | Generate Prometheus scrape and alert configurations |
| alertmanager-config | Generate Alertmanager configuration |

## Dynamic Inventory

The AWS EC2 inventory plugin discovers running EC2 instances in `ap-south-1`.

Instances are grouped using their AWS tags, including Environment and Role.

Worker instances must have the `Role=worker` tag to match the configured playbook target group.

## Setup

Install the required collections:

```bash
ansible-galaxy collection install -r requirements.yml
```

Configure AWS credentials securely before using dynamic inventory.

Review discovered hosts:

```bash
ansible-inventory --graph
```

Check playbook syntax:

```bash
ansible-playbook -i localhost, playbooks/site.yml --syntax-check
```

## Execution

After verifying the inventory and target worker nodes:

```bash
ansible-playbook playbooks/site.yml
```

## Monitoring

The repository generates configurations for:

- Prometheus metrics scraping
- Kubernetes monitoring alerts
- Alertmanager notifications

Set `SLACK_WEBHOOK_URL` securely through the environment before generating Alertmanager configuration.

## Related Repositories

- [Infrastructure Platform](https://github.com/purnima202/k8s-gitops-platform)
- [GitOps Configuration](https://github.com/purnima202/k8s-gitops-config)

## Project Status

Automation code and configuration templates are prepared. Execution against live AWS EKS worker nodes has not yet been performed.