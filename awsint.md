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
