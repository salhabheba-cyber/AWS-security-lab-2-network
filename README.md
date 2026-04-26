# 🔐 Network Security & Monitoring (3-Tier VPC)

## 📋 Overview

This lab demonstrates a production-ready 3-tier VPC architecture with defense-in-depth security controls on AWS.

### What I Built

| Component | Configuration |
|-----------|---------------|
| **VPC** | `production-vpc` with CIDR `10.0.0.0/16` |
| **Subnets** | 3 (Public: `10.0.1.0/24`, App: `10.0.2.0/24`, DB: `10.0.3.0/24`) |
| **Internet Gateway** | `prod-igw` attached to VPC |
| **Route Tables** | 3 (public-rt, app-rt, db-rt) |
| **Security Groups** | 4 (alb-sg, web-sg, bastion-sg, db-sg) |
| **Network ACLs** | 2 (web-nacl, db-nacl) |
| **VPC Flow Logs** | Enabled to CloudWatch Logs |
| **Bastion Host** | `t2.micro` in public subnet |
| **GuardDuty** | Enabled for threat detection |


## 📸 Screenshots

| # | Description |
|---|-------------|
| 1 | VPC details showing CIDR `10.0.0.0/16` |
| 2 | 3 subnets with their CIDR blocks |
| 3 | Internet Gateway attached to VPC |
| 4 | Route tables (public-rt with route to IGW) |
| 5 | 4 Security Groups configured |
| 6 | NACL inbound rules (defense in depth) |
| 7 | VPC Flow Logs configuration |
| 8 | Bastion host running with public IP |
| 9 | GuardDuty enabled |
| 10 | Internet connectivity test from Bastion host |

*(Screenshots are in the `screenshots/` folder)*

## 🛡️ Security Controls Implemented

### Security Groups (Instance-Level)

| SG Name | Inbound Rules | Source |
|---------|---------------|--------|
| `alb-sg` | HTTP (80), HTTPS (443) | `0.0.0.0/0` |
| `web-sg` | HTTP (80) | `alb-sg` |
| `bastion-sg` | SSH (22) | `[MY-IP]/32` |
| `db-sg` | PostgreSQL (5432) | `web-sg` |

### Network ACLs (Subnet-Level) - Defense in Depth

**Web Tier NACL (`web-nacl`):**

| Rule # | Type | Source | Action |
|--------|------|--------|--------|
| 100 | HTTP | `0.0.0.0/0` | Allow |
| 110 | HTTPS | `0.0.0.0/0` | Allow |
| 120 | SSH | `[MY-IP]/32` | Allow |
| 130 | All Traffic | `10.0.0.0/16` | Allow |
| 900 | All Traffic | `0.0.0.0/0` | Deny |

**DB Tier NACL (`db-nacl`):**

| Rule # | Type | Source | Action |
|--------|------|--------|--------|
| 100 | PostgreSQL | `10.0.2.0/24` | Allow |
| 900 | All Traffic | `0.0.0.0/0` | Deny |

## 🔍 Monitoring & Detection

| Service | Purpose | Status |
|---------|---------|--------|
| **VPC Flow Logs** | Capture all IP traffic | Enabled to CloudWatch |
| **GuardDuty** | Threat detection | Enabled |
| **CloudTrail** | API audit logging | Enabled  |

## 🧪 Test Results

| Test | Expected | Actual | Status |
|------|----------|--------|--------|
| Bastion Host internet access | HTTP works | `curl google.com` returned HTTP 200 | ✅ PASS |
| SSH access restricted | Only my IP | Configured with `/32` | ✅ PASS |
| Private subnets isolation | No direct internet | Route tables have no IGW | ✅ PASS |


## 🎯 Skills Demonstrated

- ✅ VPC design with CIDR planning
- ✅ 3-tier network segmentation
- ✅ Security Groups (stateful, instance-level)
- ✅ NACLs (stateless, subnet-level) - Defense in depth
- ✅ Internet Gateway + Route Tables configuration
- ✅ VPC Flow Logs for network monitoring
- ✅ Bastion host deployment for secure access
- ✅ GuardDuty threat detection
- ✅ AWS Free Tier cost management


