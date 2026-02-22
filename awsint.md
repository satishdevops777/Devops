# AWS


## EBS
- EBS - Persistent, Backup supported
- Instance Store temporary, ultrafast

***An EBS Snapshot is a backup of an EBS volume.***
- Stored internally in S3
- Incremental (only changes are saved after first snapshot)
- Used for backup & disaster recovery
- Can create new volumes from snapshot


## Highly durable (11 9’s durability)?
- 11 9s durability means that AWS guarantees 99.999999999% probability that your data will not be lost over a year. It is achieved through multi-AZ replication, redundancy, and integrity checks. It measures protection against data loss, not service uptime.
- 11 9s durability is technically strong, but alone it is NOT enough for compliance.
- You also need:
  - Versioning
  - Encryption
  - Logging
  - Cross-region strategy (sometimes)
  - Access control policies


## What Is Our RPO?
- RPO = Recovery Point Objective
- 👉 How much data loss can we tolerate?

| RPO       | Meaning                 |
| --------- | ----------------------- |
| 0 minutes | No data loss allowed    |
| 1 hour    | Can lose 1 hour of data |
| 24 hours  | Daily backup is enough  |

If:
- RPO = 0 → need replication (real-time sync)
- RPO = 1 hour → hourly backups enough
- RPO = 24 hours → daily snapshot fine

- Durability doesn’t define RPO.
- Backup frequency defines RPO.


RTO = Recovery Time Objective
(How fast system must recover)
- Maximum acceptable downtime after a failure.
- The maximum amount of time your system can be down after a failure before it causes unacceptable business impact.
  

***How Architecture Affects RTO***

- RTO depends on your design:

**1️⃣ Backup-Based Recovery (Slow)**
- Restore from snapshot
- Launch new server
- Attach volume
- RTO = Hours

**2️⃣ Multi-AZ Setup (Faster)**
- Load balancer
- Auto failover
- RTO = Minutes

**3️⃣ Multi-Region Active-Active (Very Fast)**
- Traffic routed automatically
- Secondary region already running
- RTO = Seconds

***RTO is the maximum acceptable downtime for a system after a failure. 
It defines how quickly a service must be restored to avoid business impact. Architecture design directly affects RTO — the more redundancy and automation we have, the lower the RTO.***

🛡️ Security Best Practices
- ✔ Encrypt snapshots
- ✔ Restrict snapshot sharing
- ✔ Use KMS keys
- ✔ Monitor via CloudTrail

Durability = Probability that your data will NOT be lost

## EBS Lifecycle Manager
- officially called Data Lifecycle Manager (DLM) in Amazon Web Services, is a service that automates creation, retention, and deletion of EBS snapshots.

👉 Instead of manually creating backups, DLM does it automatically based on policies.

## EFS 
- Amazon EFS (Elastic File System) is a fully managed file storage service in Amazon Web Services.
- 👉 It provides shared file storage that multiple servers (EC2 instances) can access at the same time.

### 🔎 How EFS Works
- It is regional (not limited to one AZ)
- Automatically replicated across multiple AZs
- Accessed using NFS protocol
- Scales automatically (no storage limit to manage)

| Feature      | EFS          | EBS             |
| ------------ | ------------ | --------------- |
| Storage Type | File storage | Block storage   |
| Sharing      | Multiple EC2 | Usually 1 EC2   |
| AZ Scope     | Multi-AZ     | Single AZ       |
| Scaling      | Automatic    | Manual resize   |
| Use Case     | Shared files | OS / DB storage |

| Mode        | Auto Scale | Predictable | Credit System | Best Use Case              |
| ----------- | ---------- | ----------- | ------------- | -------------------------- |
| Bursting    | Limited    | Medium      | Yes           | Small workloads            |
| Provisioned | No         | High        | No            | Consistent heavy workloads |
| Elastic     | Yes        | High        | No            | Variable workloads         |


## KMS
- KMS = Service that securely manages encryption keys in AWS.
- It is used to encrypt:
  - EBS volumes
  - S3 buckets
  - RDS databases
  - EFS file systems
  - Secrets
  - Application data

- A KMS key (CMK – Customer Managed Key) is:
- A logical representation of a cryptographic key stored securely in AWS hardware security modules (HSMs).

- Instead of encrypting large data directly with KMS:
  - 1️⃣ KMS creates a Data Encryption Key (DEK)
  - 2️⃣ DEK encrypts your actual data
  - 3️⃣ DEK itself is encrypted using the KMS master key
  - 4️⃣ Encrypted DEK stored with data

- When decrypting:
  - KMS decrypts DEK
  - DEK decrypts data
  - This is called Envelope Encryption.

- You pay for:
  - Number of keys
  - API requests (Encrypt/Decrypt)
  - Custom key usage

***AWS KMS is a managed key management service that allows secure creation, storage, and control of encryption keys. It uses envelope encryption to protect data and integrates with AWS services like S3, EBS, and RDS. It provides centralized control, auditing, and automatic key rotation.***

## Secrets Manager
- Secrets Manager = Secure vault for sensitive credentials.
- Secrets Manager solves this by:
- ✔ Encrypting secrets using AWS Key Management Service
- ✔ Automatic rotation
- ✔ IAM-based access control
- ✔ Audit logging

**AWS Secrets Manager is a managed service that securely stores and rotates sensitive credentials such as database passwords and API keys. It encrypts secrets using KMS, provides fine-grained IAM access control, and supports automatic rotation to improve security and compliance.**

**Secrets rotation in AWS Secrets Manager is performed using a Lambda function that generates a new credential, updates the target system (like RDS), updates the stored secret, and validates it. Secrets are encrypted using KMS via envelope encryption, where a data encryption key encrypts the secret and is itself encrypted with a master KMS key.**

## S3
- Amazon S3 (Simple Storage Service) is an object storage service from
Amazon Web Services.
- 👉 It stores and retrieves any amount of data from anywhere over the internet.
- S3 = Highly durable, scalable cloud storage.

### 🔥 Why S3 Is Powerful
- ✔ 11 9s durability (99.999999999%)
- ✔ Infinite scalability
- ✔ Pay only for what you use
- ✔ Multi-AZ redundancy
- ✔ Highly secure
- ✔ Integrates with almost every AWS service

### 📦 S3 Storage Classes (Cost Optimization)

| Storage Class        | Use Case             |
| -------------------- | -------------------- |
| Standard             | Frequently accessed  |
| Intelligent-Tiering  | Unpredictable access |
| Standard-IA          | Infrequent access    |
| One Zone-IA          | Non-critical data    |
| Glacier Instant      | Rare access          |
| Glacier Flexible     | Archive              |
| Glacier Deep Archive | Long-term backup     |

### 🔄 S3 Lifecycle Policies
- You can automatically:
- Move objects to cheaper storage
- Delete old objects
- Transition to Glacier after 30 days

Example:
```
After 30 days → Move to IA  
After 90 days → Move to Glacier  
After 365 days → Delete
```

### 🔐 Security in S3

- S3 security works at multiple layers:
- 1️⃣ IAM Policies: Control who can access S3.
- 2️⃣ Bucket Policies: Resource-based policy controlling access to bucket.
- 3️⃣ Encryption: 
  - SSE-S3 (AWS managed)
  - SSE-KMS (KMS managed)
  - Client-side encryption
- 4️⃣ Block Public Access: Prevents accidental public exposure.
- 5️⃣ Versioning
  - Protects against:
    - Accidental deletion
    - Overwrite
    - Ransomware
    
  ***Amazon S3 is a highly durable, scalable object storage service used for storing files, backups, logs, and application data. It supports multiple storage classes for cost optimization, integrates with IAM and KMS for security, and provides lifecycle policies for automated data management.***
