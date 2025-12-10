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
