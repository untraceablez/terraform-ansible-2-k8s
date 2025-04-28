# `terraform-ansible-2-k8s`

## What *is* this project?

The goal of this project is to provide a start to finish deployment experience of [Kubernetes](https://kubernetes.io/) (particularly k8s) on [Proxmox](https://proxmox.com/en/) via [Terraform/OpenTofu](https://opentofu.org/) and [Ansible](https://docs.ansible.com/). While there are other projects that aim to do the same thing, often times the Terraform and Ansible parts are split apart. This aims to use Ansible to not only provision Kubernetes onto the VMs, but to push the Terraform configs onto our Proxmox cluster. 

## Requirements

Before you go cloning this repo and hoping it's a 1-click solution, you should check that your lab meets the following requirements: 

| Requirement  | Default  | Reasoning   |
|---|---|---|
| Proxmox Version  | 8+  | I have no way to test previous revisions of Proxmox.  |
| Kubernetes Version  | v.133.0  | Latest stable release as of writing  |
| # of Proxmox Nodes  | 3  | High-availability kubernetes should be distributed across physical nodes. You can run this on one node, but you won't have any true HA, just simulated within your cluster. |
| Development Machine  | Debian-based Machine w/ Ansible & Terraform (or OpenTofu) installed. | You'll need an independent machine **not** running on the cluster to run your Ansible commands.  |
| Networking | Ability to create and manage VLANs on network. | You'll want a cluster subnet dedicated to k8s, as well as a regular LAN address likely. |

## How it Works

This Ansible repository works by utilizing 3 roles to deploy your k8s environment. 

* `dev`: This role installs all necessary dependencies for your development machine where this repository has been cloned. **This role always runs first.**

* `terraform`: This role deploys the k8s cluster to Proxmox using Terraform.

* `k8s`: This role installs k8s and it's dependencies on your freshly created VMs.


