# AWS Networking – Interview Questions & Answers

A complete GitHub-compatible Markdown file for AWS Networking interview prep.

---

## 1. VPC Basics

### **Q1. What is a VPC?**

A Virtual Private Cloud is an isolated virtual network in AWS where you control subnets, routing, security, and IP ranges.

### **Q2. What is CIDR notation?**

CIDR defines the IP block (e.g., `10.0.0.0/16`).

- `/16` → 65,536 IPs
- `/24` → 256 IPs

### **Q3. Public vs Private Subnet**

- **Public:** Route table has `0.0.0.0/0 → IGW`
- **Private:** No IGW route; uses NAT Gateway for outbound

### **Q4. Internet Gateway (IGW)**

Enables internet access for resources in public subnets.

### **Q5. NAT Gateway**

Allows **private** subnet instances to access the internet without being exposed.

### **Q6. Security Groups vs NACLs**

- **SG:** Stateful, instance-level
- **NACL:** Stateless, subnet-level

### **Q7. Route Table**

Controls where subnet traffic goes (IGW, NAT, TGW, endpoints, etc).

### **Q8. Reserved IPs in subnets**

AWS reserves **5 IPs** per subnet:

1. Network address
2. VPC router
3. AWS DNS
4. Future use
5. Broadcast

### **Q9. How does a private EC2 reach the internet?**

Via **NAT Gateway** in a public subnet.

### **Q10. What are VPC Endpoints?**

Private access to AWS services.

- **Gateway:** S3, DynamoDB
- **Interface:** Most other services

---

## 2. Intermediate Networking

### **Q1. EC2 cannot reach internet — Troubleshooting Checklist**

- Public IP/EIP assigned?
- Subnet route: `0.0.0.0/0 → IGW`
- IGW attached to VPC?
- SG outbound allows 0.0.0.0/0?
- NACL outbound open?
- OS firewall?
- Source/destination check disabled if required?

### **Q2. Access S3 privately**

Use **Gateway VPC Endpoint for S3**.

### **Q3. What is VPC Peering? Limitations?**

Peer-to-peer private connection.  
**Limitations:**

- No transitive routing
- No overlapping CIDRs

### **Q4. Transit Gateway vs VPC Peering**

- **Peering:** One-to-one
- **TGW:** Hub-and-spoke, transitive routing supported

### **Q5. How to build a highly available NAT setup?**

Create **one NAT Gateway per AZ** and route private subnets to local NAT.

### **Q6. Why is DB unreachable across VPCs?**

Possible causes:

- SG rules
- NACL rules
- Missing routes
- Overlapping CIDRs
- Wrong TGW/peering config

### **Q7. SG vs NACL (return traffic)**

- SG: Stateful → return allowed
- NACL: Stateless → explicit allow needed

### **Q8. Can NAT forward VPC-to-VPC traffic?**

No — NAT is only for internet egress.

### **Q9. How to restrict S3 to a VPC?**

Use S3 bucket policy with:

- `aws:SourceVpc`
- or `aws:SourceVpce`

### **Q10. Route 53 Routing Policies**

Latency, Weighted, Failover, Geolocation, Geoproximity, Multi-value.

---

## 3. Advanced Networking

### **Q1. API Gateway vs ALB vs NLB**

| Service     | Layer | Best for                                |
| ----------- | ----- | --------------------------------------- |
| API Gateway | L7    | APIs, throttling, auth, transformations |
| ALB         | L7    | Web apps, host/path routing             |
| NLB         | L4    | High throughput, static IPs, TCP/UDP    |

### **Q2. Multi-region multi-account architecture**

Use **AWS Organizations + TGW + Shared Services VPC + Central Egress VPC**.

### **Q3. What is AWS PrivateLink?**

Private service-to-service connectivity using interface endpoints (ENIs). No public exposure.

### **Q4. Direct Connect vs Site-to-Site VPN**

- **DX:** Stable, high throughput
- **VPN:** Cheaper, quick, internet-based

### **Q5. Designing dev/staging/prod VPCs**

Use separate AWS accounts or isolated VPCs.  
Use TGW/PrivateLink to interconnect when required.

### **Q6. Centralized logging**

Use VPC Flow Logs → CloudWatch/S3 → Org-level config.

### **Q7. Global Accelerator vs CloudFront vs Route 53**

- **GA:** Accelerates TCP/UDP traffic
- **CloudFront:** CDN caching
- **Route 53:** DNS routing only

### **Q8. Cross-account private access**

Use **PrivateLink** or **TGW + RAM**.

### **Q9. When prefer NLB?**

For extreme performance, static IPs, non-HTTP traffic.

### **Q10. TGW scaling limits?**

Use multi-TGW architecture, segmentation, and spreading VPC attachments.

---

## 4. Scenario Questions & Answers

### **Scenario 1: Design a secure 3-tier architecture**

- Public subnets → ALB
- Private subnets → App EC2
- Private subnets → RDS
- NAT for outbound updates
- SG flow: ALB → App → DB

### **Scenario 2: Migrate on-prem to AWS without changing IPs**

Use **Direct Connect + Transit Gateway + BYOIP + hybrid DNS**.

### **Scenario 3: All outbound traffic must be firewall inspected**

Use **Inspection VPC + Gateway Load Balancer (GWLB)**.

### **Scenario 4: Central NAT for multiple accounts**

Use **TGW + Shared Services VPC + NAT Gateways in shared VPC**.

### **Scenario 5: SMTP from private subnets**

Use SES with VPC endpoint or private SMTP relay.

---

## 5. Hands-on / Practical Questions (with answers)

### **Q1. Build VPC with public/private subnets**

Create:  
VPC → Subnets → IGW → NAT → Route tables.

### **Q2. Test S3 using endpoint**

Use S3 Gateway Endpoint and perform AWS CLI S3 operations without internet.

### **Q3. Configure VPC Peering**

Create peering → Accept → Update route tables on both ends.

### **Q4. PrivateLink cross-account**

Service owner creates Endpoint Service → Consumer creates Interface Endpoint.

### **Q5. Fix security issue (SSH open to world)**

- Remove `0.0.0.0/0`
- Enforce SSM
- SCP restriction
- Continuous monitoring

---
