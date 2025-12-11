# AWS IAM - Quick Notes + Interview Q&A

## What is IAM?

- IAM = **Identity and Access Management**
- Global AWS service for **authentication and authorization**
- Manage **Users, Groups, Roles, Policies**
- Ensures **least privilege access** to resources

---

## IAM Users

- Represents **individuals or applications**
- Credentials:
  - **Password** → AWS Console
  - **Access Key & Secret Key** → CLI/SDK
- Each user is unique
- Avoid using **root account** except for account-level tasks

---

## IAM Groups

- Collection of IAM users
- Assign **permissions collectively**
- Users can belong to **multiple groups**
- Groups cannot contain other groups
- Examples: `Admins`, `Developers`, `ReadOnly`

---

## IAM Roles

- Temporary access identity with **no permanent credentials**
- Can be assumed by:
  - AWS services (EC2, Lambda)
  - IAM users (cross-account access)
  - Federated users (SSO/AD)
- Common use cases:
  - EC2 accessing S3
  - Lambda writing logs to CloudWatch
  - CI/CD pipelines assuming roles

---

## IAM Policies

- JSON documents defining **Allow/Deny** permissions
- Can be attached to Users, Groups, Roles
- Types:
  - **Managed Policies**: AWS or customer managed
  - **Inline Policies**: Attached directly to a single identity
- Elements:
  - `Effect`: Allow or Deny
  - `Action`: API operations (e.g., `s3:GetObject`)
  - `Resource`: ARN of the resource
  - `Condition`: Optional filters (tags, IP, MFA)

---

## Types of Access Control

### Identity-Based Policies

- Attached to **users, groups, or roles**
- Define what the identity can perform

### Resource-Based Policies

- Attached directly to AWS resources (S3, SNS, SQS)
- Allow **cross-account access** without roles

### Permission Boundaries

- Restrict the **maximum permissions** an identity can get
- Useful for developer safety

### Service Control Policies (SCPs)

- Used in **AWS Organizations**
- Restrict entire accounts
- Cannot grant permissions, only restrict

---

## Temporary Security Credentials (STS)

- Provides **short-lived credentials** for temporary access
- APIs:
  - `AssumeRole`
  - `GetSessionToken`
  - `AssumeRoleWithWebIdentity`
- Used for cross-account access, federated users, CI/CD pipelines

---

## IAM Evaluation Logic

- Permission evaluation order:
  1. **Explicit Deny** → highest priority
  2. **Allow**
  3. Default deny
- **Explicit deny overrides allow**

---

## Best Practices

- Apply **least privilege**
- Prefer **IAM roles over hard-coded credentials**
- Enable **MFA** for sensitive accounts
- Rotate or avoid **access keys**
- Use **IAM Access Analyzer** to detect unintended public access
- Regularly audit permissions

---

## Common Interview Questions + Answers

### Q1: Difference between User, Group, and Role

- **User:** Individual identity with credentials
- **Group:** Collection of users for shared permissions
- **Role:** Temporary credentials, can be assumed by users/services

### Q2: Managed vs Inline Policies

- **Managed Policy:** Reusable, AWS or customer managed
- **Inline Policy:** Embedded directly in one identity, not reusable

### Q3: How EC2 Instance Profiles work

- Roles are assigned to EC2 via **Instance Profiles**
- EC2 automatically receives temporary credentials
- Avoid storing static access keys on instances

### Q4: Resource-based vs Identity-based Policy

- **Identity-based:** Attached to IAM user/group/role
- **Resource-based:** Attached directly to AWS resources
- Resource-based can allow **cross-account access**

### Q5: Cross-account access with roles

- Create a **role in Account B**
- Give **trust** to Account A
- Users/services in Account A **assume the role** temporarily

### Q6: What is a permission boundary?

- Max permissions an IAM entity can receive
- Useful to **restrict developers from giving themselves extra permissions**

### Q7: Why explicit deny is stronger than allow

- Explicit deny **overrides all allows**
- Ensures sensitive resources are protected even if multiple policies allow access
