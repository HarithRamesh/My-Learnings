# AWS EC2 – Interview Questions & Answers

## 1. What is Amazon EC2?

**Answer:**  
Amazon EC2 (Elastic Compute Cloud) is a service that provides scalable virtual servers (instances) in the cloud. You can choose CPU, memory, storage, and networking capacity and pay only for what you use.

---

## 2. What are EC2 instance types?

**Answer:**  
Instance types define hardware configuration. Categories include:

- **General Purpose (t3, m5)** – balanced CPU/memory.
- **Compute Optimized (c5, c6g)** – better for compute-heavy workloads.
- **Memory Optimized (r5, x1e)** – high memory workloads.
- **Storage Optimized (i3, d2)** – high IOPS needs.
- **Accelerated Computing (p3, g4)** – GPU/ML workloads.

---

## 3. What are EC2 purchasing options?

**Answer:**

- **On-Demand:** Pay hourly/second, no commitment.
- **Reserved Instances:** 1/3-year commitment, big savings.
- **Savings Plans:** Flexible commitment for compute usage.
- **Spot Instances:** Up to 90% cheaper; AWS can reclaim anytime.
- **Dedicated Hosts:** Physical servers dedicated to one customer.

---

## 4. What is an AMI?

**Answer:**  
An Amazon Machine Image is a template used to launch EC2 instances. It contains OS, application server, packages, and configurations. You can use AWS-provided, Marketplace, or custom AMIs.

---

## 5. What is the difference between Stop vs Terminate an EC2 instance?

**Answer:**

- **Stop:** Shuts down the OS; EBS volumes stay; instance can restart.
- **Terminate:** Deletes the instance and usually deletes EBS root volume (unless disabled).

---

## 6. What happens to the IP address when an instance is stopped?

**Answer:**  
Public IP is released. Private IP remains. Explicit Elastic IPs stay attached.

---

## 7. What are Security Groups?

**Answer:**  
Virtual firewalls that control **inbound and outbound traffic** at instance level.

- Only **allow** rules (no deny rules).
- Stateful: return traffic is automatically allowed.

---

## 8. What are NACLs (Network ACLs)?

**Answer:**  
Stateless firewalls at subnet level.

- Support **allow and deny** rules.
- Return traffic is NOT automatically allowed.

---

## 9. When should you use Security Groups vs NACLs?

**Answer:**

- **Security Groups:** Instance-level security — use most of the time.
- **NACLs:** Additional layer of subnet-level protection.

---

## 10. What is EC2 Auto Scaling?

**Answer:**  
Automatically adjusts the number of EC2 instances based on demand. Supports scaling policies like CPU usage, request count, schedules, and target tracking.

---

## 11. What is a Placement Group?

**Answer:**  
Logical grouping of instances to influence placement:

- **Cluster:** Close together for low-latency, high network throughput.
- **Spread:** Instances placed on different hardware to reduce failure risk.
- **Partition:** Instances split into partitions for large distributed systems.

---

## 12. What are EBS volume types?

**Answer:**

- **gp3:** General purpose SSD.
- **io1/2:** High-performance SSD for databases.
- **st1:** Throughput-optimized HDD.
- **sc1:** Cold HDD for infrequent access.

---

## 13. What is the difference between EBS and Instance Store?

**Answer:**

- **EBS:** Persistent block storage, survives stop/start.
- **Instance Store:** Temporary storage tied to host hardware; data lost when instance stops.

---

## 14. What is Elastic IP Address?

**Answer:**  
A static IPv4 address you control. It remains until manually released. Useful for stable public IPs.

---

## 15. What is User Data in EC2?

**Answer:**  
A script that runs on instance boot (one time by default). Often used for installing software, updates, or configuration automatically.

---

## 16. What is EC2 Instance Metadata?

**Answer:**  
Info available inside the instance at URL:  
`http://169.254.169.254/latest/meta-data/`  
Contains instance ID, IAM role, hostname, etc.

---

## 17. What are EC2 Instance Roles?

**Answer:**  
IAM roles assigned to EC2 instances to securely grant permissions without storing AWS credentials.

---

## 18. What is Elastic Load Balancing?

**Answer:**  
Distributes traffic across multiple EC2 instances. Works with auto scaling. Types: ALB, NLB, CLB.

---

## 19. How do you achieve high availability with EC2?

**Answer:**

- Deploy across multiple AZs
- Use Auto Scaling
- Use Load Balancer
- Use healthy AMIs and snapshots

---

## 20. What is EC2 Hibernate?

**Answer:**  
Preserves the in-memory state, saves it to EBS, and resumes quickly without rebooting. Useful for long initialization workloads.

---

## 21. What is the difference between EC2 and Lambda?

**Answer:**

- **EC2:** Server-based, long-running, customizable OS.
- **Lambda:** Serverless, event-driven, auto-scaled, short-running functions.

---

## 22. How does EC2 billing work?

**Answer:**  
Based on:

- Instance runtime
- Storage (EBS)
- Data transfer
- Load balancers
- Elastic IP (if unattached)

---

## 23. How do you monitor EC2 instances?

**Answer:**  
Using CloudWatch for:

- CPU, network, disk
- Custom metrics
- Logs via CloudWatch Logs
- Health checks via ELB

---

## 24. What are EC2 Host Tenancy types?

**Answer:**

- **Shared:** Default, cheapest.
- **Dedicated Instance:** Single-tenant but flexible.
- **Dedicated Host:** Fully dedicated hardware.

---

## 25. What is the difference between Public, Private, and Elastic IP?

**Answer:**

- **Public IP:** Auto-assigned, changes on stop/start.
- **Private IP:** Permanent within VPC.
- **Elastic IP:** Static, user-controlled.

---

## 26. How to ensure EC2 instance can connect to the internet?

**Answer:**  
Requirements:

- Public IP or Elastic IP
- Route table with route to **Internet Gateway**
- Security group outbound rules
- NACL allows outbound traffic

---

## 27. Can an EC2 instance have multiple IPs?

**Answer:**  
Yes, via **secondary private IPs** and **multiple Elastic IPs** (one per secondary private IP).

---

## 28. What is an ENI (Elastic Network Interface)?

**Answer:**  
A network card attached to EC2. Supports:

- Multiple IPs
- Security groups
- Attachment/detachment between instances

---

## 29. What is EC2 Instance Connect?

**Answer:**  
A secure, browser-based SSH access method using temporary keys instead of permanent ones.

---

## 30. What is Capacity Reservation?

**Answer:**  
Guarantees EC2 compute capacity in a specific AZ for compliance or critical workloads.
