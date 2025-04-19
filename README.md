# devops-static-site

A fully automated “static site” deployment pipeline on DigitalOcean, powered by Terraform, Ansible and Jenkins.

## Table of Contents

- [Overview](#overview)  
- [Features](#features)  
- [Prerequisites](#prerequisites)  
- [Architecture](#architecture)  
- [Getting Started](#getting-started)  
  - [1. Register Your Domain](#1-register-your-domain)  
  - [2. Provision Infrastructure with Terraform](#2-provision-infrastructure-with-terraform)  
  - [3. Bootstrap Servers with Ansible](#3-bootstrap-servers-with-ansible)  
  - [4. Configure DNS & HTTP](#4-configure-dns--http)  
  - [5. Configure Jenkins Pipeline](#5-configure-jenkins-pipeline)  
- [Repository Layout](#repository-layout)  
- [License](#license)  

---

## Overview

This project demonstrates a simple yet end‑to‑end DevOps workflow to host a static website:

1. **Domain registration** on GoDaddy.  
2. **Droplet creation** on DigitalOcean via Terraform.  
3. **Server provisioning** with Ansible (Apache, BIND DNS, Jenkins).  
4. **Zone & HTTP configuration** with Ansible playbooks.  
5. **Automated HTML deployment** through a Jenkins pipeline.

All infrastructure as code (IaC) and configuration management is stored in this single Git repository.

---

## Features

- **Terraform** to spin up and tear down DigitalOcean droplets.  
- **Ansible** playbooks to install and configure:
  - Apache (httpd)  
  - BIND DNS server  
  - Jenkins CI  
  - Service restarts  
- **Ansible** to deploy your DNS zone file and your website’s HTTP configuration.  
- **Jenkinsfile** to build and deploy your static site (index.html) on commit.

---

## Prerequisites

- A **GoDaddy** account (or any DNS registrar) with a purchased domain.  
- A **DigitalOcean** account and API token.  
- **Terraform** (v1.0+) installed locally.  
- **Ansible** (v2.9+) installed locally.  
- **Jenkins** (v2.300+) for CI/CD (can be self‑hosted in this project).  
- **Git** to clone this repository.

---

## Architecture

```plaintext
[GoDaddy DNS]            [DigitalOcean API]
      │                          │
      └───> Terraform ──> Droplet(s) ──> Ansible ──> Install & Configure
                                               ├─ Apache (HTTP)
                                               ├─ BIND (DNS)
                                               ├─ Jenkins (CI)
                                               └─ Service Restarts
                                                            │
                                                            └──> Jenkins Pipeline ──> Deploy site (index.html)
```plaintext
## Getting Started
1. Register Your Domain
Log in to GoDaddy (or your preferred registrar).

Purchase and note the domain name (e.g. example.com).

In your registrar’s dashboard, point the NS records to your future BIND server (you’ll update these after provisioning).

2. Provision Infrastructure with Terraform
