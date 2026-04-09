# Infra

This directory contains the **Infrastructure as Code (IaC)** for the platform:  
VPC/VNet networking, Kubernetes control planes (EKS/AKS/on‑prem), node groups, databases, bastions and edge nodes.

Terraform defines the cloud/on‑prem resources; Ansible applies Linux hardening and node‑level configuration.

## Structure

```text
infra/
  terraform/
    global/
      backend/
        s3-state.tf
        dynamodb-locks.tf
    modules/
      network-aws/
      network-azure/
      eks/
      aks/
      rds-postgres/
      redis/
      bastion/
      edge-node/
    environments/
      aws-dev/
        main.tf
        variables.tf
      aws-test/
        main.tf
      aws-pilot/
        main.tf
      aws-prod/
        main.tf
      azure-dev/
        main.tf
      azure-prod/
        main.tf
      edge-lab/
        main.tf
  ansible/
    inventories/
      dev/
        hosts.ini
      prod/
        hosts.ini
      edge/
        hosts.ini
    roles/
      base-linux-hardening/
      k8s-node-setup/
      microshift-install/
      db-server-hardening/
      observability-agent/
    playbooks/
      provision-k8s-nodes.yml
      harden-linux.yml
      microshift-bootstrap.yml
      edge-fleet-patch.yml
      db-provision-and-harden.yml
```

## Purpose

- **Terraform**:
  - Create and manage networking (VPC/VNet, subnets, routing, security groups/NSGs).
  - Provision EKS/AKS/on‑prem clusters, node groups, RDS/DBs, caches, bastions and edge VMs.
  - Separate reusable `modules/` from per‑environment `environments/`.

- **Ansible**:
  - Apply **Linux hardening** aligned with CIS/ISM baselines to all nodes.
  - Install and configure Kubernetes/MicroShift components on worker and edge nodes.
  - Roll out observability agents, security tooling and OS‑level updates across the fleet.

> In short: `infra/` is responsible for **how** the platform is physically and virtually built, while `clusters/` defines **which** Kubernetes clusters exist and how they are organised for GitOps and fleet management.