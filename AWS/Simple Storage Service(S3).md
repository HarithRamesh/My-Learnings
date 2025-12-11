# Amazon S3 - Quick Notes

## What is Amazon S3?

- S3 stands for **Simple Storage Service**
- Fully-managed, scalable **object storage** service
- Store and retrieve any amount of data from anywhere
- Supports web access through **REST APIs**
- Common for media files, backups, static websites, data lakes

---

## Storage Structure

- S3 stores data in **Buckets**
- Buckets contain **Objects**
- Objects can be any type of data: documents, images, videos, audio, etc.

---

## S3 Object Components

| Component           | Description                                                 |
| ------------------- | ----------------------------------------------------------- |
| Key                 | Unique name/identifier of the object in a bucket            |
| Version ID          | Unique version identifier (if versioning is enabled)        |
| Value               | The actual data (file content)                              |
| Metadata            | Additional info such as content-type, size, encoding        |
| Subresources        | Extra settings like ACL, Lifecycle rules                    |
| Access Control Info | Permissions determining who can access or modify the object |

---

## Key Features

- **11 9's durability**: 99.999999999%
- Data stored across multiple Availability Zones
- **Versioning** supports recovery from accidental deletions or modifications
- **Lifecycle policies** move objects to cheaper storage classes automatically
- **Encryption**: SSE-S3, SSE-KMS, and HTTPS for data protection
- Supports **Static Website Hosting**
- **Event Notifications** integrate with Lambda, SQS, or SNS for automation

---

## Common Use Cases

- Static website hosting
- Backup and disaster recovery
- Media storage and streaming
- Application asset storage
- Data lake for analytics workloads

---

## Quick Facts

- Global service (Bucket names must be **unique globally**)
- Unlimited total storage capacity
- Object size: **0 bytes to 5 TB**
- Large files use **Multipart Upload**

| Feature        | Object Storage                              | Block Storage                               |
| -------------- | ------------------------------------------- | ------------------------------------------- |
| Data Structure | Stored as objects (data + metadata + ID)    | Stored in fixed-sized blocks                |
| Performance    | Higher latency, optimized for large content | Low latency, high performance               |
| Scalability    | Highly scalable (virtually unlimited)       | Limited to a single instance or volume size |
| Metadata       | Rich metadata support                       | Minimal metadata                            |
| Use Cases      | Backups, media, logs, static content        | Databases, OS disks, high-IO apps           |
| Access Method  | REST API (HTTP)                             | Attached via filesystem (like a disk)       |
| Cost           | Lower cost for large data                   | More expensive                              |
| Durability     | High redundancy across regions/AZs          | Depends on volume configuration             |

# Amazon S3 Storage Classes

S3 provides multiple storage classes optimized for different performance, cost, and access patterns. The goal is to store data at the **lowest cost** based on how frequently it is accessed.

---

## 1️⃣ S3 Standard

- Default storage class
- Designed for **frequently accessed** data
- Stored redundantly across **multiple AZs**
- High durability and availability

**Use cases:**

- User content (images, videos)
- Application data
- Website content

---

## 2️⃣ S3 Intelligent-Tiering

- Automatically moves data between tiers based on usage
- No performance impact, no retrieval charges
- Small monthly monitoring fee per object

**Use cases:**

- Unknown or changing access patterns

---

## 3️⃣ S3 Standard-IA (Infrequent Access)

- Lower cost than Standard storage
- Data is accessed **less frequently**
- Retrieval cost applies
- Stored across **multiple AZs**

**Use cases:**

- Backups
- Disaster recovery data

---

## 4️⃣ S3 One Zone-IA

- Same as Standard-IA but stored in **one AZ**
- Cheaper, but **less resilient**
- If AZ fails, data can be lost

**Use cases:**

- Re-creatable data
- Secondary backups

---

## 5️⃣ S3 Glacier Instant Retrieval

- Very low cost
- **Milliseconds retrieval**
- Suitable for long-term data needing immediate access

**Use cases:**

- Medical records
- Archived data with random access needs

---

## 6️⃣ S3 Glacier Flexible Retrieval (formerly Glacier)

- Low-cost archival storage
- Minutes to hours retrieval options
- No need for instant access

**Use cases:**

- Backup & archive where access is rare

---

## 7️⃣ S3 Glacier Deep Archive

- **Cheapest** storage class
- Retrieval time: **12–48 hours**
- Best for long-term retention (compliance data)

**Use cases:**

- Regulatory and compliance archives
- Cold data retained for years

---

## Comparison Table

| Storage Class              | Access Pattern | Availability | Retrieval Time | Cost     | Use Case                      |
| -------------------------- | -------------- | ------------ | -------------- | -------- | ----------------------------- |
| Standard                   | Frequent       | Highest      | Immediate      | High     | Active data                   |
| Intelligent-Tiering        | Changing       | High         | Immediate      | Medium   | Unknown access pattern        |
| Standard-IA                | Infrequent     | High         | Immediate      | Low      | Backups, DR                   |
| One Zone-IA                | Infrequent     | Lower        | Immediate      | Lower    | Re-creatable data             |
| Glacier Instant Retrieval  | Rare           | High         | Milliseconds   | Very Low | Archives needing quick access |
| Glacier Flexible Retrieval | Rare           | High         | Minutes–hours  | Very Low | Long-term backup              |
| Glacier Deep Archive       | Very rare      | Medium       | 12–48 hrs      | Lowest   | Compliance storage            |

# S3 Bucket Policies vs IAM Policies

IAM and Bucket Policies are both used to control access to S3 objects, but they work at **different scopes and purposes**.

---

## IAM Policies

- **Identity-based** (applied to Users, Groups, and Roles)
- Define _what an IAM principal can access_
- Stored and managed in **IAM**
- Can grant access to **multiple services**, including S3
- Evaluated when the principal makes a request

**Example:**
Grant a specific user permission to upload objects into a bucket.

---

## S3 Bucket Policies

- **Resource-based** (attached directly to S3 buckets)
- Define _who can access the bucket and objects_
- Stored and managed at the **bucket level**
- Can grant access to external AWS accounts or public users
- Often used to:
  - Enable public/static website access
  - Grant cross-account permissions

**Example:**
Allow anonymous read access for website hosting.

---

## Key Differences

| Feature                  | IAM Policy                             | S3 Bucket Policy             |
| ------------------------ | -------------------------------------- | ---------------------------- |
| Type                     | Identity-based                         | Resource-based               |
| Scope                    | Users/Groups/Roles                     | S3 buckets & objects         |
| Can allow public access? | No                                     | Yes                          |
| Cross-account access     | Limited (needs roles)                  | Simple, direct configuration |
| Primary use case         | Permission for a specific user/service | Access rules for the bucket  |

---

## How AWS Determines Access (Evaluation Logic)

1. **Explicit Deny** → Always wins
2. **Explicit Allow** (via IAM or Bucket policy)
3. If no allow → access is denied by default

So:

- Both policies can apply at the same time
- If one has Deny → request fails

---

## Permission Flow Example

User tries to download an object:

- Check IAM policy → is the action allowed?
- Check Bucket policy → is the user or public allowed?
- Any explicit Deny → Reject request

---

## Access Control List (ACL)

- Legacy feature
- Granular object-level permissions
- _Not recommended_ unless:
  - **Cross-account** object ownership issues
  - Bucket owner and object uploader differ (e.g., logs delivery)

---

## When to Use What?

| Situation                            | Best Option            |
| ------------------------------------ | ---------------------- |
| Give AWS service access to bucket    | IAM Role               |
| Public static website on S3          | Bucket Policy          |
| Cross-account resource sharing       | Bucket Policy          |
| Individual developer/app permissions | IAM Policy             |
| Object-level permissions             | ACL (only if required) |

---

## Recommended Security Best Practices

- Enable **Block Public Access** unless explicitly required
- Prefer **IAM policies** for internal access
- Enable **S3 logging + CloudTrail** for auditing
- Use **SSE-KMS** for encryption
- Use VPC Endpoints to keep S3 traffic inside AWS network

- **S3 Standard**

  - Frequently accessed data
  - High availability and multi-AZ redundancy

- **S3 Intelligent-Tiering**

  - Automatically moves objects between frequent/rare access tiers
  - No performance impact
  - Small monitoring fee

- **S3 Standard-IA (Infrequent Access)**

  - Lower-cost storage for infrequently accessed data
  - Retrieval costs apply
  - Multi-AZ redundancy

- **S3 One Zone-IA**

  - Same as Standard-IA but stored in **one AZ**
  - Cheaper, but less resilient

- **S3 Glacier**

  - Archival storage
  - Minutes-to-hours retrieval

- **S3 Glacier Deep Archive**
  - Lowest-cost storage
  - Retrieval time: 12–48 hours

Understand differences in:

- Cost
- Retrieval time
- Durability vs. availability
- Suitable use cases

---

## 2. S3 Data Consistency

- S3 provides **strong read-after-write consistency** for all PUT, DELETE, and overwrite operations.
- Applies across all AWS Regions.

---

## 3. S3 Encryption Options

To protect data at rest:

- **SSE-S3 (Server-Side Encryption, S3 managed keys)**

  - Keys fully managed by S3

- **SSE-KMS (KMS managed keys)**

  - More secure
  - Key usage costs
  - Audit logs via CloudTrail

- **SSE-C (Customer-managed keys)**

  - You supply your own encryption keys

- **Client-side encryption**
  - Data encrypted before upload

---

## 4. S3 Versioning

- Stores multiple versions of the same object.
- Helps recover from accidental deletions or overwrites.
- Once enabled, cannot be disabled—only suspended.
- Can be combined with **MFA Delete** for stronger protection.

---

## 5. Bucket Policies & IAM

Two main methods to control access:

- **Bucket Policy (resource-based)**

  - Attached directly to the bucket
  - Grants access to users, roles, or public
  - Used for public hosting, cross-account access

- **IAM Policy (identity-based)**
  - Attached to users, groups, or roles
  - Defines what the identity can access

Other:

- **ACLs** — legacy, used only when needed for object-level permissions.

---

## 6. S3 Access Points

- Each access point provides a **unique endpoint** to access a bucket.
- Useful for large datasets accessed by multiple applications.
- Allows fine-grained, application-specific access control.

---

## 7. S3 Public Access Blocking

- AWS blocks all public access **by default** using:
  - Block Public ACLs
  - Block Public Bucket Policies
  - Ignore Public ACLs
  - Restrict Public Buckets

To host public static files:

- You must manually allow public access using a bucket policy.

---

## 8. Static Website Hosting

To host a static website on S3:

1. Enable **Static Website Hosting**
2. Upload `index.html` and optional `error.html`
3. Add a **public-read bucket policy**
4. Access via S3 website endpoint (not the S3 REST endpoint)

Often combined with **CloudFront** for HTTPS + caching.

---

## 9. S3 Event Notifications

S3 can send notifications when objects change:

- **Lambda**

  - Trigger serverless functions for processing data

- **SQS**

  - Queue-based processing workflows

- **SNS**
  - Push notifications (email, other services)

Common use cases:

- Generate thumbnails after image upload
- Trigger ETL pipeline
- Notify application of new logs

---

## 10. S3 Performance & Optimization

- **Multipart Upload**

  - Required for files > 100MB
  - Recommended for > 5GB
  - Faster, resilient uploads

- **S3 Transfer Acceleration**

  - Upload via AWS edge locations
  - Useful for global uploads

- **Byte-Range Fetching**
  - Download specific portions of a file
  - Useful for media streaming or resuming downloads

---

## 11. Security & Monitoring

- **S3 Access Logs**

  - Logs requests made to the bucket

- **CloudTrail**

  - Tracks API calls (PUT, GET, DELETE)

- **Object Lock**

  - Write Once Read Many (WORM)
  - Ensures objects cannot be modified or deleted
  - Used for compliance workloads

- **Requester Pays**
  - Data download costs are paid by the requester
  - Useful for public datasets

---

## 12. Data Protection Features

- **Cross-Region Replication (CRR)**

  - Automatically replicates objects across AWS Regions
  - Helps with compliance and disaster recovery
  - Bucket versioning must be enabled

- **Same-Region Replication (SRR)**
  - Copies objects across buckets within the same region
  - Used for logs, analytics, or security pipelines

# S3 Interview Questions and Answers

## 1️⃣ How do you secure S3 data from public access?

- Enable **Block Public Access** at bucket and account level (default ON)
- Use **IAM Policies** and **Bucket Policies** to allow only authorized access
- Encrypt data using SSE-S3, SSE-KMS, or client-side encryption
- Use **VPC Endpoints** to keep traffic inside AWS network
- Enable **MFA Delete** when versioning is on (for extra protection)
- Monitor access using **CloudTrail** and **S3 Access Logs**

---

## 2️⃣ How to serve a static React app using S3 + CloudFront?

1. Build the React project (`npm run build`)
2. Upload the build folder to an S3 bucket
3. Enable **Static Website Hosting** in S3
4. Set `index.html` as the index document
5. Make objects public using a bucket policy
6. Use **CloudFront** as a CDN for better performance and HTTPS
7. In CloudFront, set the S3 bucket as the origin

Result:

- Low latency, global caching, secure HTTPS delivery

---

## 3️⃣ How to reduce cost of old or rarely accessed data?

Use **Lifecycle Policies**:

- Move data to **S3 Standard-IA** after X days
- Archive to **Glacier** or **Deep Archive** after Y days
- Optionally **delete** after Z days

This automates storage **cost optimization**.

---

## 4️⃣ When to use Glacier instead of S3 Standard-IA?

| Requirement                               | Recommended               |
| ----------------------------------------- | ------------------------- |
| Access frequency is low, but not archival | Standard-IA               |
| Data is accessed **very rarely**          | Glacier                   |
| Long-term, compliance storage             | Glacier Deep Archive      |
| Retrieval time matters (< few minutes)    | Glacier Instant Retrieval |

So choose **Glacier** for:

- Backups
- Compliance data
- Digital archives

---

## 5️⃣ Explain S3 consistency model.

S3 provides **strong read-after-write** and **strong consistency** for:

- PUT (new objects)
- DELETE (existing objects)
- Overwrites — latest version always returned

This applies in **all AWS Regions**.

---

## 6️⃣ What happens if someone deletes a versioned object?

- Object is **not removed**
- Only a **delete marker** is added
- Previous versions are still recoverable
- To permanently delete:
  - Delete specific versions manually or via lifecycle rules
- Enabling **MFA Delete** makes permanent deletion more secure

This protects against accidental data loss.

---
