

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

## Route53
- Amazon Route 53 is a highly available and scalable DNS (Domain Name System) service from Amazon Web Services.
- 👉 It translates domain names into IP addresses.

- Route 53 provides 3 main capabilities:
  - 1️⃣ Domain Registration
  - 2️⃣ DNS Management
  - 3️⃣ Health Checking & Traffic Routing

### 🏗️ Hosted Zones
- A Hosted Zone is a container for DNS records.

***🔹 Public Hosted Zone***
- Accessible from internet.

***🔹 Private Hosted Zone***
- Works inside VPC only.

| Record | Purpose              |
| ------ | -------------------- |
| A      | Maps domain → IPv4   |
| AAAA   | Maps domain → IPv6   |
| CNAME  | Domain → Domain      |
| MX     | Mail server          |
| TXT    | Verification         |
| Alias  | AWS resource mapping |

### 🚀 Routing Policies (Very Important)
- Route 53 is powerful because of smart routing.

| Routing Policy         | How It Works                                         | Use Case                         | Supports Health Check | Example Scenario                  |
| ---------------------- | ---------------------------------------------------- | -------------------------------- | --------------------- | --------------------------------- |
| **Simple**             | Routes traffic to a single resource                  | Basic domain mapping             | ❌ No                  | `example.com → ALB`               |
| **Weighted**           | Distributes traffic by percentage                    | Canary / Blue-Green deployment   | ✅ Yes                 | 80% → v1, 20% → v2                |
| **Latency-Based**      | Routes to region with lowest latency                 | Global apps                      | ✅ Yes                 | India → Mumbai, US → Virginia     |
| **Failover**           | Primary → Secondary if health fails                  | Disaster recovery                | ✅ Required            | Active-Passive DR                 |
| **Geolocation**        | Routes based on user country                         | Region-specific content          | ✅ Yes                 | US users → US server              |
| **Geoproximity**       | Routes based on geographic distance (can shift bias) | Traffic shifting between regions | ✅ Yes                 | Shift 20% traffic from EU to Asia |
| **Multi-Value Answer** | Returns multiple healthy IPs                         | Basic load balancing             | ✅ Yes                 | Multiple EC2 IPs returned         |
| **IP-Based Routing**   | Routes based on client IP CIDR                       | Enterprise routing rules         | ❌ No                  | Corporate IP → Internal endpoint  |


## Load Balancers
- A Load Balancer distributes incoming traffic across multiple servers so:
  - No single server is overloaded
  - High availability
  - Fault tolerance

- Types:
  - ALB (Application Load Balancer – Layer 7)
  - NLB (Network Load Balancer – Layer 4)
  - CLB (Legacy)
- Most common today → ALB

- Target Group = A group of backend servers that receive traffic.
  - Targets can be:
    - EC2 instances
    - IP addresses
    - Lambda functions
    - EKS pods (via IP mode)
   
### ❤️ Health Checks
- ALB performs health checks
  ```
  GET /health
  ```
- This ensures:
  - ✔ No traffic sent to unhealthy servers
  - ✔ Automatic recovery

### Auto Scaling automatically:
- Adds servers when traffic increases
- Removes servers when traffic decreases
- Service: Amazon EC2 Auto Scaling

### How Load Balancer + Auto Scaling Work Together
- Step-by-step:
  - 1️⃣ Traffic increases
  - 2️⃣ CPU crosses threshold
  - 3️⃣ Auto Scaling launches new EC2
  - 4️⃣ New EC2 registers automatically in Target Group
  - 5️⃣ ALB starts sending traffic to new instance

- When traffic drops:
  - 1️⃣ Auto Scaling terminates instance
  - 2️⃣ Instance deregistered from Target Group
  - 3️⃣ ALB stops sending traffic

- Completely automatic.

### 🔥 Important Concepts
- Deregistration Delay
- When instance removed:
  - ALB waits (default 300 seconds)
  - To finish active connections.
  - Prevents dropped requests.
- Sticky Session (also called Session Affinity) means:
  - A user’s requests are always sent to the same backend server for a period of time.
  - Instead of load balancer sending each request to any healthy server, it “sticks” a user to one specific target.
  - This feature exists in
    - Elastic Load Balancing (especially ALB and CLB).

 
***A Load Balancer distributes incoming traffic across multiple backend targets grouped in a Target Group. Auto Scaling dynamically adjusts the number of backend instances based on metrics such as CPU utilization. When new instances are launched, they are automatically registered with the load balancer’s target group, and when terminated, they are deregistered to ensure seamless traffic distribution and high availability.***

## Amazon API Gateway 
- Amazon API Gateway  is a fully managed service from Amazon Web Services
that allows you to:
- Create APIs (REST / HTTP / WebSocket)
- Secure them
- Throttle them
- Monitor them
- Integrate them with backend services
- It gives:
  - ✔ Authentication
  - ✔ Rate limiting
  - ✔ Logging
  - ✔ Monitoring
  - ✔ Caching
  - ✔ Security

  | Feature                | API Gateway | ALB      |
| ---------------------- | ----------- | -------- |
| Layer                  | 7           | 7        |
| Rate limiting          | Yes         | Limited  |
| API Keys               | Yes         | No       |
| Authentication         | Advanced    | Basic    |
| Serverless integration | Native      | Indirect |
| Cost                   | Higher      | Lower    |

## CDN (Content Delivery Network)
- A CDN (Content Delivery Network) is a globally distributed network of servers that: Delivers content to users from the nearest geographic location.
- CDN improves:
  - ✔ Speed
  - ✔ Performance
  - ✔ Global availability
  - ✔ Security
  - ✔ Reduced backend load
 
| Feature         | CDN       | ALB      |
| --------------- | --------- | -------- |
| Global presence | Yes       | Regional |
| Caching         | Yes       | No       |
| Static content  | Excellent | No       |
| Dynamic routing | Limited   | Yes      |
| Used for APIs   | Sometimes | Yes      |

***A CDN is a globally distributed network that caches content at edge locations closer to users to reduce latency and improve performance. In AWS, CloudFront is used as a CDN and can sit in front of S3, ALB, or API Gateway to accelerate content delivery and enhance security.***

## 1️⃣ AWS Organization & Landing Zone Design
### Architecture Design
```
AWS Organization
│
├── Management Account
├── Security Account
├── Log Archive Account
├── Shared Services Account
├── Dev Account
├── QA Account
└── Prod Account
```

- Why Multi-Account?
  - ✔ Blast radius reduction
  - ✔ Environment isolation
  - ✔ Compliance separation
  - ✔ Centralized governance
 
### 🛡️ SCP Guardrails
- Apply SCPs:
  - Deny disabling CloudTrail
  - Deny deleting backups
  - Restrict root usage
  - Restrict public S3

## 2️⃣ Landing Zone (Foundation)
- Use AWS Control Tower or custom baseline.
- Landing Zone Includes:
  - ✔ Centralized CloudTrail
  - ✔ GuardDuty
  - ✔ Security Hub
  - ✔ Config Rules
  - ✔ IAM baseline roles
  - ✔ Central logging bucket
  - ✔ KMS governance
- Security & compliance built from day 1.

## 3️⃣ Network Architecture (Highly Available)
- In Prod Account:
- VPC Design
  - 1 VPC per region
  - 3 Availability Zones
  - Public subnets (ALB)
  - Private subnets (App)
  - DB subnets (isolated)
  - NAT Gateway per AZ
  - No single AZ dependency.

## 4️⃣ Application Architecture (HA + Scalable)
```
Flow:

User
↓
CloudFront
↓
WAF
↓
ALB (Multi-AZ)
↓
EKS / Auto Scaling EC2
↓
RDS (Multi-AZ)
↓
S3
```
## 5️⃣ Multi-Region DR Strategy
- Now we design DR.
  - Primary Region: ap-south-1
  - Secondary Region: ap-southeast-1

## DR Options
| Strategy         | RTO       | RPO       | Cost      |
| ---------------- | --------- | --------- | --------- |
| Backup & Restore | Hours     | Hours     | Low       |
| Pilot Light      | 1–2 hours | Minutes   | Medium    |
| Warm Standby     | 5–15 mins | Near zero | High      |
| Active-Active    | Seconds   | Zero      | Very High |



## 6️⃣ Cost Optimization Strategy
- Enterprise cost control includes:
- Compute
  - ✔ Use Reserved Instances / Savings Plans
  - ✔ Auto Scaling
  - ✔ Spot instances for non-prod
  - ✔ Right-size using Compute Optimizer
  - 
- Storage
  - ✔ S3 lifecycle policies
  - ✔ EBS gp3 instead of gp2
  - ✔ Snapshot lifecycle management

- Database
  - ✔ Right-size RDS
  - ✔ Use Aurora serverless if applicable
  - ✔ Read replicas only when needed

## 7️⃣ RPO & RTO Definition (Enterprise Example)
- Let’s define business requirement:
  - RTO = 15 minutes
  - RPO = 5 minutes

- Design decisions:
  - ✔ Multi-AZ DB
  - ✔ Cross-region read replica
  - ✔ Route 53 failover
  - ✔ TTL = 60 seconds
  - ✔ Infrastructure as Code

## 🔐 8️⃣ Security & Compliance
- ✔ KMS encryption everywhere
- ✔ Secrets Manager
- ✔ IAM least privilege
- ✔ WAF + Shield
- ✔ VPC endpoints (private access)
- ✔ Central log archive

## 9️⃣ Observability & SRE Layer
  - ✔ CloudWatch metrics
  - ✔ Centralized logging
  - ✔ Alarms on 5xx errors
  - ✔ Synthetic monitoring
  - ✔ Chaos testing

## 10️⃣ Blast Radius Reduction
- ✔ Separate accounts
- ✔ SCP guardrails
- ✔ Separate KMS keys per account
- ✔ No cross-environment access

**To design a highly available and cost-optimized AWS environment, I start with AWS Organizations and a multi-account landing zone for governance and isolation. I implement multi-AZ infrastructure using ALB and Auto Scaling for compute and RDS Multi-AZ for databases. For disaster recovery, I deploy a warm standby in a secondary region with cross-region replication and Route 53 failover routing. I define RTO and RPO targets and align the DR strategy accordingly while continuously optimizing cost through Savings Plans, lifecycle policies, and right-sizing.**

## Final Enterprise Architecture Layers

| Layer        | Strategy                  |
| ------------ | ------------------------- |
| Organization | Multi-account             |
| Security     | SCP + Central logging     |
| Network      | Multi-AZ                  |
| Compute      | Auto Scaling              |
| Database     | Multi-AZ + Replica        |
| DR           | Warm standby              |
| DNS          | Route 53 failover         |
| Cost         | Savings Plans + lifecycle |
| Monitoring   | CloudWatch + alarms       |


## CNAPP - Cloud Native Application Protection Platform

### It is a unified security platform that protects:
- Cloud infrastructure
- Containers
- Kubernetes
- Serverless
- IAM
- Data
- CI/CD pipelines

- Instead of separate tools for each layer, CNAPP combines:

| Security Area          | Included |
| ---------------------- | -------- |
| CSPM                   | Yes      |
| CWPP                   | Yes      |
| CIEM                   | Yes      |
| IaC scanning           | Yes      |
| Vulnerability scanning | Yes      |
| Container scanning     | Yes      |

## CSPM = Cloud Security Posture Management
- Focuses on:
  - Misconfigurations in cloud accounts

Example issues:
- Public S3 buckets
- Open Security Groups (0.0.0.0/0)
- Unencrypted EBS
- Disabled CloudTrail
- Weak IAM policies
- CSPM checks against:
- CIS Benchmarks: NIST, SOC2, PCI-DSS, GDPR

- It scans via:
  - ✔ Cloud APIs
  - ✔ Read-only IAM access
  - No agents required.

## CWPP = Cloud Workload Protection Platform
- Focuses on:
  - Runtime workload security
  - Protects: EC2, Containers, Kubernetes nodes, Serverless
  - Detects: Vulnerabilities, Malware, Privilege escalation, Suspicious processes, Crypto mining, Kernel exploits
  - CWPP may use:
    - ✔ Agents
    - ✔ Agentless scanning
    - ✔ Runtime monitoring

## TCP/IP – Core Networking Foundation
- TCP/IP – Core Networking Foundation

## IP (Layer 3 – Routing)
- Identifies a host (e.g., 10.0.1.15)
- Used in VPC CIDR blocks (e.g., 10.0.0.0/16)
- Routing tables decide where packets go

## TCP (Layer 4 – Reliable Transport)
- Connection-oriented
- 3-way handshake (SYN → SYN-ACK → ACK)
- Used by HTTP, HTTPS, SSH, databases

##🔹 UDP
- No handshake
- Used by DNS, streaming, monitoring

| Problem                          | Root Cause               |
| -------------------------------- | ------------------------ |
| Timeout                          | SG/NACL blocked          |
| Refused                          | App not listening        |
| 502                              | Unhealthy backend        |
| 504                              | Backend slow             |
| Cannot assume role               | Trust relationship issue |
| Works internally, not externally | SG public access missing |
| Cross-VPC failure                | Route table missing      |

***I would start by identifying whether the issue is network, application, or authorization related, then systematically isolate layers beginning with DNS and load balancer, moving through security groups and routing, and finally verifying IAM permissions and application logs.***
