# Amazon Web Services (AWS)

**AWS Regions**
- AWS has Regions all around the world.
- Names can be us-east-1, eu-west-3…
- A region is a cluster of data centers
- Most AWS services are region-scoped

**✅ Factors to Consider When Choosing an AWS Region**

**1. Latency & Proximity to Users**
- Pick a region closest to your end users or business operations.
- Lower network latency → better performance (important for customer-facing apps).
- Example: If most users are in India → choose ap-south-1 (Mumbai).
- Proximity = how physically close the AWS Region (data center) is to your end users, applications, or your company’s own infrastructure.

**2. Compliance & Data Residency**
- Some industries (banking, healthcare, government) require data to remain in a specific country.
- Example:
  - EU GDPR compliance → use eu-central-1 (Frankfurt) or eu-west-1 (Ireland).
  - Indian data residency laws → Mumbai Region.

**3. Service Availability**
- Not all AWS services are available in every region.
- Example:
  - New services (like Bedrock, Q, or Local Zones) usually launch first in us-east-1 (N. Virginia).
  - If you need Outposts or Wavelength Zones, check AWS Region availability.
👉 Always check the AWS Regional Services List before finalizing.

**4. Cost**
- Prices differ by region.
- Example:
  - Running an EC2 instance in us-east-1 is usually cheaper than in ap-south-1.
  - If latency/compliance isn’t strict → you can optimize cost by picking a cheaper region.

**5. Disaster Recovery & Multi-Region Strategy**
- For high availability:
  - Choose a primary region close to your users.
  - Use a secondary region for DR (Disaster Recovery).
- Example:
  - Primary: Mumbai
  - DR: Singapore

**6. Ecosystem & Partner Support**
- Some AWS Regions have better connectivity to Direct Connect locations, CloudFront POPs, and enterprise partners.
- Example: If your corporate data center is in Singapore, hosting in ap-southeast-1 reduces data transfer latency.

**7. Legal & Tax Implications**
- Some regions have different billing structures or legal frameworks.
- Enterprises may choose a region based on legal risk minimization.

**AWS Availability Zones**
- Each region has many availability zones (usually 3, min is 3, max is 6). Example:
  - ap-southeast-2a
  - ap-southeast-2b
  - ap-southeast-2c
- Each availability zone (AZ) is one or more discrete data centers with redundant power, networking, and connectivity
- They’re separate from each other, so that they’re isolated from disasters
- They’re connected with high bandwidth, ultra-low latency networking

**AWS Point of Presence**
- A PoP (Edge Location) is a smaller AWS data center that’s part of the Amazon CloudFront CDN (Content Delivery Network).
- Instead of hosting your entire app, it mainly caches content and provides faster access to users nearby.
- It reduces latency by serving requests closer to users, without routing everything back to the main AWS Region.

**📌 Types of PoPs**
- Edge Locations
  - Used for caching static content (images, videos, JS, CSS, etc.) via CloudFront.
  - Also used for services like AWS Global Accelerator, Route 53 (DNS), and Shield (DDoS protection).
- Regional Edge Caches
  - Larger caches located between Edge Locations and AWS Regions.
  - They hold less frequently accessed content for longer, reducing traffic back to the Region.

**⚡ How PoPs Work (Example)**
- Imagine your app is hosted in us-east-1 (Virginia), but your users are in India:
- Without PoPs → Requests go all the way to Virginia = ~250 ms latency.
- With a Mumbai Edge Location → The content is cached at the nearest PoP, so user requests are served in 30–50 ms.

**✅ Services Using PoPs**
- Amazon CloudFront (CDN) → Caching and delivery of web content.
- Amazon Route 53 → DNS queries answered faster from local PoPs.
- AWS Shield & WAF → DDoS and security protections at the edge.
- Global Accelerator → Directs traffic through AWS global network for reduced latency.

👉 In short:
- Region = where your workloads run.
- Edge Location (PoP) = where your content is delivered from, close to the user.

## 📘 1. AWS Identity and Access Management (AWS IAM)
**IAM: Users & Groups**
- IAM = Identity and Access Management, Global service
- Root account created by default, shouldn’t be used or shared
- Users are people within your organization, and can be grouped
- Groups only contain users, not other groups
- Users don’t have to belong to a group, and user can belong to multiple groups

**IAM: Permissions**
- Permissions define what actions are allowed or denied for a user, group, or role.
- Permissions in IAM are assigned using policies.
- A policy is a JSON document that specifies:
  - Effect → Allow / Deny
  - Action → Which API calls are allowed (e.g., s3:PutObject, ec2:StartInstances)
  - Resource → Which AWS resource the action applies to (e.g., a specific bucket or instance)

**Types of Policies**

- AWS Managed Policies 
  - Predefined by AWS.
  - Example: AmazonS3ReadOnlyAccess, AdministratorAccess.
  - Good for quick setup.

- Customer Managed Policies
  - Created and managed by you.
  - More flexible, reusable.
  - Example: Create a policy to allow EC2 start/stop only.

- Inline Policies
  - Attached directly to a single user, group, or role.
  - Not reusable.
  - Best avoided in most cases.

🔐 IAM Policy Structure
- An IAM Policy is a JSON document that defines permissions. It has a fixed structure:

**1. Top-Level Elements**
- Version
  - Defines the policy language version.
  - Always use "2012-10-17" (latest & recommended).

- Id (optional)
  - Identifier for the policy.
  - Useful for tracking/logging.

- Statement (required)
  - One or more permission rules.
  - Each statement defines a single permission or restriction.

**2. Inside a Statement**
- Each Statement block has these elements:
  - Sid (optional) → Statement ID (label for readability).
  - Effect → "Allow" or "Deny".
  - Principal → The who the policy applies to (only in resource-based policies, e.g., S3 bucket policy).
  - Action → The what actions are allowed/denied (e.g., s3:PutObject, ec2:StartInstances).
  - Resource → The where (which resources). Defined using ARNs.
  - Condition (optional) → Extra restrictions (e.g., only allow from specific IPs, time of day, MFA required).

```json

{
  "Version": "2012-10-17",
  "Id": "S3CombinedPolicy",
  "Statement": [
    {
      "Sid": "ListAllBuckets",
      "Effect": "Allow",
      "Action": "s3:ListAllMyBuckets",
      "Resource": "*"
    },
    {
      "Sid": "ReadObjectsFromBucket",
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-example-bucket/*"
    },
    {
      "Sid": "AllowAccountUpload",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::111122223333:root"
      },
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::my-example-bucket/*"
    },
    {
      "Sid": "IPRestrict",
      "Effect": "Allow",
      "Action": "s3:*",
      "Resource": "arn:aws:s3:::secure-bucket/*",
      "Condition": {
        "IpAddress": {
          "aws:SourceIp": "203.0.113.0/24"
        }
      }
    }
  ]
}
```
**📝 Description of This Policy**

- ListAllBuckets
  - Allows the IAM user/group/role to list all S3 buckets in the account.

- ReadObjectsFromBucket
  - Grants read access (s3:GetObject) to objects in the bucket my-example-bucket.

- AllowAccountUpload
  - Lets another AWS account (111122223333) upload (s3:PutObject) to your bucket my-example-bucket.
  - Uses Principal because it’s a resource-based statement.

- IPRestrict
  - Allows full S3 actions (s3:*) on the bucket secure-bucket but only if the request comes from IP range 203.0.113.0/24.
  - Uses Condition to enforce network-based restriction.

**⚠️ Note:**
- In real AWS, you don’t normally mix identity-based and resource-based policies into one JSON, since they’re attached to different places (IAM user vs S3 bucket).
- But for learning/demo purposes, this combined policy shows all main components (Allow/Deny, Principal, Resource, Condition).

**IAM – Password Policy**
- Strong passwords = higher security for your account
  - In AWS, you can setup a password policy:
  - Set a minimum password length
  - Require specific character types:
    - including uppercase letters
    - lowercase letters
    - numbers
    - non-alphanumeric characters
  - Allow all IAM users to change their own passwords
  - Require users to change their password after some time (password expiration)
  - Prevent password re-use

**1. What is MFA?**
- MFA = Multi-Factor Authentication → an extra layer of security for signing in.
- A user must provide two things:
  - Something you know → password.
  - Something you have → MFA device (code/token).
- This makes it much harder for attackers to access your account, even if the password is stolen.

**2. Where is MFA Used in AWS?**
- Root account → must always enable MFA (best practice).
- IAM users → can (and should) enable MFA for extra security.
- Roles (via MFA-Required policies) → you can require MFA before assuming a sensitive role.

**3. MFA Devices Supported**

- AWS supports different MFA devices:
  - Virtual MFA Device (most common)
    - Uses an app on a phone (Google Authenticator, Authy, Microsoft Authenticator, AWS Virtual MFA).
    - Generates a 6-digit code every 30 seconds.
    - Free and easy.

  - Hardware MFA Device
    - A physical device (key fob) from vendors like Gemalto.
    - Used by enterprises with strict compliance needs.

  - U2F Security Key (FIDO2)
    - USB/NFC hardware key (like YubiKey).
    - Plug it in or tap it to authenticate.

  - SMS Text Message MFA (legacy, less secure)
    - AWS used to support SMS-based MFA, but it’s being phased out for higher security options.

**4. IAM Policy with MFA Requirement**
- You can enforce MFA for certain API calls. Example: require MFA to delete objects in S3:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowAllS3ActionsWithMFA",
      "Effect": "Allow",
      "Action": "s3:*",
      "Resource": "*",
      "Condition": {
        "Bool": {
          "aws:MultiFactorAuthPresent": "true"
        }
      }
    }
  ]
}
```
- If MFA is required to assume a role:
```json
aws sts get-session-token \
  --serial-number arn:aws:iam::123456789012:mfa/johndoe \
  --token-code 123456


# --serial-number → ARN of your MFA device.

# --token-code → 6-digit code from the MFA app/device.
```
- Returns temporary credentials for secure use.

✅ In short: 
- MFA = extra security layer for AWS accounts. 
- Enable it for root + IAM users, enforce it with policies, and use it with CLI/roles when needed.
---
# IAM - Architect View
---

| Identity Type      | Used For                     | Architect Concern    |
| ------------------ | ---------------------------- | -------------------- |
| IAM User           | Legacy / specific automation | Avoid overuse        |
| IAM Role           | Workloads, cross-account     | Preferred            |
| Federated Identity | Enterprise users             | SSO integration      |
| Root User          | Account ownership            | Lock down completely |

## Policies (Where Architecture Matters)
- There are 4 important policy layers you design:
  - Identity-based policies
  - Resource-based policies
  - Permission boundaries
  - SCPs (Service Control Policies via AWS Organizations)

## Multi-Account IAM Architecture (Enterprise Design)

```vbnet
AWS Organization
│
├── Management Account
├── Security Account
├── Shared Services Account
├── Dev Account
├── QA Account
└── Prod Account

```

- Isolation, Blast radius control, Compliance separation, Environment segregation

## IAM Role Strategy


### 🔹 Workload Roles
  - EC2 Role
  - Lambda Role
  - ECS Task Role
  - No access keys stored.

## 🔹 Cross-Account Roles
- Used for:
  - Dev → Access Shared Services
  - Security → Audit All Accounts
  - CI/CD → Deploy to Prod
  - Uses Trust Policy

  ```json
  {
  "Effect": "Allow",
  "Principal": {
    "AWS": "arn:aws:iam::123456789012:root"
  },
  "Action": "sts:AssumeRole"
  }
  ```
## Federation & Enterprise Identity
- As architect, you integrate AWS with:
  - Active Directory
  - Azure AD
  - Okta
  - SAML Providers
  - OIDC
- Now AWS users don’t exist locally → they login via SSO.
- Modern approach: 👉 AWS IAM Identity Center
- This replaces large IAM user management.

## IAM Security Design Principles

### 🔐 Principle of Least Privilege
- Only required actions allowed.

### 🔐 Zero Trust Approach
- Every request must be authenticated & authorized.

### 🔐 Separation of Duties
- Dev cannot modify billing
- Ops cannot change IAM policies
- Security has read-only audit

### 🔐 Defense in Depth
- Use: IAM, SCP, Permission boundaries, Resource policies, CloudTrail logging

## Governance Controls You Must Design

| Control               | Why                                                  |
| --------------------- | ---------------------------------------------------- |
| SCPs                  | Prevent dangerous actions (e.g., disable CloudTrail) |
| Permission Boundaries | Limit power of IAM creators                          |
| Tag-based IAM         | ABAC (Attribute Based Access Control)                |
| MFA Enforcement       | Secure human access                                  |
| Access Analyzer       | Detect over-permissions                              |

### Architect-Level IAM Blueprint Summary

| Layer      | What You Design                 |
| ---------- | ------------------------------- |
| Identity   | SSO via Identity Center         |
| Accounts   | Multi-account via Organizations |
| Workloads  | IAM Roles only                  |
| Governance | SCP + Boundaries                |
| Audit      | Central CloudTrail              |
| Security   | Least Privilege + MFA           |


| Layer               | Attached To | Grants Permission? | Restricts? | Scope           |
| ------------------- | ----------- | ------------------ | ---------- | --------------- |
| Identity Policy     | User/Role   | ✅ Yes              | ❌ No       | Single identity |
| Resource Policy     | Resource    | ✅ Yes              | ❌ No       | That resource   |
| Permission Boundary | User/Role   | ❌ No               | ✅ Yes      | That identity   |
| SCP                 | Account/OU  | ❌ No               | ✅ Yes      | Entire account  |

```pgsql
SCP (Outer Wall)
    ↓
Permission Boundary (Team Limit)
    ↓
Identity Policy (What user is allowed)
    ↓
Resource Policy (What resource allows)
```
- If ANY says Deny → Final = Deny.

### 🚀 Final Architect Advice
- Identity policy = Normal permission
- Resource policy = Cross-account & special cases
- Permission boundary = Delegation control
- SCP = Enterprise governance


## RDS Proxy

Amazon RDS Proxy is a fully managed database proxy service for Amazon RDS and Amazon Aurora. It helps improve application scalability, resiliency, and security by sitting between your application and your RDS/Aurora databases. RDS Proxy is never publically accessible(Must be accessed from VPC)

**What is RDS Proxy?**
Imagine your application opens and closes many database connections rapidly. This can:
- Exhaust your DB instance resources (especially under heavy load).
- Cause performance bottlenecks.

**Amazon RDS Proxy helps by:**
- Pooling and reusing existing database connections.
- Managing connection limits.
- Automatically handling failovers in multi-AZ deployments.

Your app connects to RDS Proxy instead of directly to the RDS database.
```javascript
App ↔ RDS Proxy ↔ RDS/Aurora DB
```
- RDS Proxy keeps a pool of open connections to the database.
- Your app’s connections are routed through the proxy.
- Connections are reused efficiently.
- During failovers, the proxy maintains app connections while switching to the new DB node.

✅ Benefits of RDS Proxy
  | Feature                | Benefit                                 |
  | ---------------------- | --------------------------------------- |
  | **Connection pooling** | Reduces DB connection overhead.         |
  | **High availability**  | Seamless failover support.              |
  | **IAM authentication** | Use AWS IAM for secure access.          |
  | **TLS support**        | Encrypts traffic between app and proxy. |
  | **Security**           | Integrates with AWS Secrets Manager.    |

📦 Supported Databases
  - Amazon RDS for MySQL
  - Amazon RDS for PostgreSQL
  - Amazon RDS for MariaDB
  - Amazon Aurora (MySQL-compatible and PostgreSQL-compatible)

🛠️ How to Set Up RDS Proxy (Console)
  - Go to RDS Console → Proxies → Create proxy
  - Choose the DB engine (MySQL/PostgreSQL).
  - Select the RDS or Aurora DB instance or cluster.
  - Choose a Secrets Manager secret for DB credentials.
  - Enable IAM authentication (optional).
  - Configure VPC, subnets, and security groups.
  - Create the proxy.

📌 When NOT to Use RDS Proxy?
  - If your app has long-lived connections and doesn’t create many new ones.
  - If your DB supports high connection counts without degradation.
  - For unsupported database engines (e.g., Oracle or SQL Server, as of now).

**Extra:**

💡 What is "Connection Pooling"?
  - Connection pooling is like having a reusable set of doors into your database, instead of building a new door every time someone wants to enter.

🖥️ In Programming Terms:
- Every time your app wants to talk to a database, it needs to:
  * Open a connection (expensive and slow)
  * Use it to run a query
  * Close the connection

Doing this repeatedly is slow and inefficient, especially under high traffic. So instead, we use a connection pool: A "pool" of open connections is kept ready.
When the app needs a connection, it borrows one from the pool. When done, it returns it to the pool — no need to close and reopen. This is faster and uses fewer resources.

🔁 **Without vs With Connection Pooling**

| Step             | Without Pooling        | With Pooling             |
| ---------------- | ---------------------- | ------------------------ |
| New connection?  | Every time             | Reuse from pool          |
| Open/close cost? | High                   | Low                      |
| App performance  | Slower under load      | Faster, more scalable    |
| Database load    | Heavy connection churn | Efficient connection use |

🔌 **Why It's Important**
  - Databases (like MySQL or PostgreSQL) can only handle limited connections.
  - Opening and closing connections frequently is slow.
  - Under high traffic, apps can crash or time out without pooling.

**Example Scenario**
**Use Case:**
A Lambda function that interacts with an RDS database can cause connection issues when it scales up, as each Lambda instance opens a new DB connection.

Solution:
Use RDS Proxy.

Architecture:
```csharp
[Lambda Function] 
       │
       ▼
[RDS Proxy (pooled connections)]
       │
       ▼
[RDS MySQL/Aurora DB]
```
**Benefit:**
- The Lambda function quickly gets a connection from the proxy.
- No connection storms during high loads.
- Automatic failover during DB maintenance or failure.

**Code Example (Node.js + MySQL)**
- Here’s how your code changes when using RDS Proxy:

```js

# With direct DB connection
const connection = mysql.createConnection({
  host: 'your-db-instance.rds.amazonaws.com',
  user: 'admin',
  password: 'password',
  database: 'mydb'
});
```
```js
`
# With RDS Proxy
const connection = mysql.createConnection({
  host: 'your-rds-proxy.proxy-region.rds.amazonaws.com',
  user: 'admin',
  password: 'password',
  database: 'mydb'
});
```

## ElastiCache

💡 **What is Amazon ElastiCache?**
Amazon ElastiCache is a fully managed in-memory caching service by AWS. It helps your applications run faster by storing frequently accessed data in memory, instead of always querying a slower database.

🧠 **Think of It Like This:**
- Without cache: Every time your app needs data, it asks the database. This is slow if it happens millions of times.
- With cache: Your app checks the cache first. If the data is there (a cache hit), it returns instantly. If not (a cache miss), it goes to the database and stores the result in the cache for next time.

🔧 **ElastiCache Supports Two Engines:**

| Engine        | Description                                                                                |
| ------------- | ------------------------------------------------------------------------------------------ |
| **Redis**     | Feature-rich cache + data store with advanced data structures (sorted sets, pub/sub, etc.) |
| **Memcached** | Simpler, high-speed caching layer, great for simple key-value use cases                    |

✅ Both are in-memory → very fast (microseconds)
✅ Fully managed by AWS → no need to set up servers


🚀 **Why Use ElastiCache?**

| Benefit                 | Explanation                                       |
| ----------------------- | ------------------------------------------------- |
| ⚡ Super fast reads      | Data served from memory, not disk                 |
| 📉 Reduce database load | Fewer queries to your main DB                     |
| ♻️ Reuse data           | Cache results that don’t change often             |
| 🔒 Secure & Scalable    | Works inside your VPC, can scale up or out easily |

🖼️ **Architecture Example**
Without ElastiCache:
```psql
User → App → Database → Slow response (due to heavy load)
```
With ElastiCache:
```sql
User → App
          ├─> Check ElastiCache → If hit → return fast result
          └─> If miss → Query DB → Save result in cache → Return
```


🛍️ **Real-world Example: E-commerce App**
**Problem:**
- You run an online store. Thousands of users are browsing the same product pages. Every time they open a page, your app:
- Queries the database for product info
- Loads slowly due to database strain

**Solution:**
Use ElastiCache (Redis):

1. When a user visits a product page:
  - App checks ElastiCache: GET product:1234

2. If cache hit: return fast

3. If cache miss:
  - Get from database
  - Store in cache: SET product:1234 {product_data} EX 3600 (cache for 1 hour)

📜 **Code Example (Python + Redis)**
```python

import redis
import psycopg2
import json

# Connect to Redis (ElastiCache Redis endpoint)
cache = redis.StrictRedis(host='my-redis-cache.xxxxxx.0001.use1.cache.amazonaws.com', port=6379, decode_responses=True)

# Connect to PostgreSQL DB
conn = psycopg2.connect(database='mydb', user='admin', password='pass', host='db.example.com')
cursor = conn.cursor()

def get_product(product_id):
    # 1. Try to get from cache
    cached = cache.get(f"product:{product_id}")
    if cached:
        return json.loads(cached)
    
    # 2. If not in cache, get from DB
    cursor.execute("SELECT * FROM products WHERE id = %s", (product_id,))
    row = cursor.fetchone()
    product = {"id": row[0], "name": row[1], "price": row[2]}

    # 3. Store in cache for 1 hour
    cache.setex(f"product:{product_id}", 3600, json.dumps(product))

    return product
```
🧪 **When Should You Use ElastiCache?**
✅ Great for:
  - Frequently read data (e.g., product catalogs, user sessions, leaderboards)
  - High-volume applications (gaming, social media, e-commerce)
  - Serverless apps (like AWS Lambda) that need faster responses

🚫 Not ideal for:
- Long-term data storage
- Highly sensitive data that shouldn’t be cached
- Cases where your data changes constantly every second

🔐 Security & Access
- ElastiCache runs inside your VPC
- You control access using security groups
- Supports encryption, authentication, and IAM policies

🎯 **Summary**
Feature	Description
| Feature            | Description                                 |
| ------------------ | ------------------------------------------- |
| **What is it?**    | In-memory cache service                     |
| **Why use it?**    | Improve speed, reduce DB load, scale easily |
| **Types?**         | Redis (advanced), Memcached (simple)        |
| **Use cases?**     | Product info, user sessions, leaderboards   |
| **Fully managed?** | Yes – AWS handles setup, scaling, failover  |

🧠 **Common Use Cases**
Scenario A: Application using RDS + ElastiCache
```yaml
VPC: my-app-vpc
 ├─ Subnet 1: App Server (EC2 or Lambda)
 ├─ Subnet 2: RDS (MySQL/PostgreSQL)
 └─ Subnet 3: ElastiCache (Redis/Memcached)
```
✅ All components in the same VPC/subnets → they can talk over internal network → fast, secure, no extra config needed

✅ Recommended Setup
   - Use the same VPC and preferably the same subnet group or availability zone for:
   - ElastiCache
   - RDS
   - Your application

✅ This gives you:
  - Low latency
  - Secure private connections
  - Simpler setup and maintenance



## Route53
1. **What it is DNS?**
- Domain Name System (DNS) is the "phonebook of the internet."
- It translates human-friendly hostnames into machine-readable IP addresses.
```scss
www.amazon.com  →  54.239.28.85
```
Without DNS, you’d have to type the IP address instead of the name.

2. **Why it’s important:**
- Backbone of the Internet: Every web request starts with DNS resolution.
- Hierarchical Naming Structure:
```scss
.com (Top-Level Domain)
     └── example.com (Second-Level Domain)
            └── www.example.com (Subdomain)
                   └── api.example.com (Nested Subdomain)
```
3. **DNS Terminologies.**
   
| Term                          | Meaning                                                               | Example                                           |
| ----------------------------- | --------------------------------------------------------------------- | ------------------------------------------------- |
| **Domain Registrar**          | Company that lets you register domain names.                          | Amazon Route 53, GoDaddy                          |
| **DNS Records**               | Mappings from names to IPs or other resources.                        | A, AAAA, CNAME, NS                                |
| **Zone File**                 | The file that stores DNS records for a domain.                        | Contains all `A`, `CNAME`, etc. for `example.com` |
| **Name Server**               | Server that answers DNS queries (Authoritative or Non-Authoritative). | ns-2048.awsdns-64.com                             |
| **TLD (Top-Level Domain)**    | Last part of the domain name.                                         | `.com`, `.in`, `.org`                            |
| **SLD (Second-Level Domain)** | Domain name under a TLD.                                              | `amazon.com`, `google.com`         |


4. **How DNS Works – Step-by-Step**
- Let’s say you want to visit www.example.com:
```
Browser:
You type the address, and the browser asks the OS, “What’s the IP for www.example.com?”

Local DNS Server (Resolver):
Usually managed by your ISP or company network. If it knows the IP from cache, it returns it immediately.

Root DNS Server:
If not cached, resolver asks the root servers (managed by ICANN).
Root server replies: “I know where .com TLD servers are. Here’s their IP.”

TLD DNS Server:
Resolver asks the .com TLD servers (managed by IANA).
They respond: “The nameservers for example.com are 5.6.7.8.”

SLD DNS Server (Authoritative):
Resolver queries the nameservers for example.com (e.g., Route 53).
Route 53 replies: “www.example.com = 9.10.1.12.”

Browser Connects:
Browser connects to 9.10.1.12 and loads the website.
```

5. **Route 53 Overview**
- Amazon Route 53 is:
-- Highly available & scalable
-- Fully managed authoritative DNS
-- Lets you update your own DNS records
-- Can check health of your endpoints
-- The only AWS service with 100% SLA uptime guarantee

6. **Route 53 Records**
- Each DNS record defines how you route traffic for a domain.
A record contains:
-- Name: example.com or app.example.com
-- Type: A, AAAA, CNAME, etc.
-- Value: IP or another hostname
-- Routing Policy: How Route 53 decides which record to use
-- TTL: How long the resolver caches the record

7. **Record Types**

| Record Type | Purpose                                              | Example                              |
| ----------- | ---------------------------------------------------- | ------------------------------------ |
| **A**       | Hostname → IPv4 address                              | `www.example.com → 192.0.2.44`       |
| **AAAA**    | Hostname → IPv6 address                              | `api.example.com → 2001:db8::1234`   |
| **CNAME**   | Hostname → another hostname (must resolve to A/AAAA) | `blog.example.com → www.example.com` |
| **NS**      | Nameservers for the hosted zone                      | `ns-2048.awsdns-64.com`              |

💡 **AWS SAA Tip:C
 - Route 53 also supports Alias records (AWS-specific) → Like CNAME but can be used for root domains and integrates with AWS resources like S3, CloudFront, ALB.

8. **Hosted Zones**
- A Hosted Zone is a container for all the records for a domain.

Two types:

- Public Hosted Zone
 -- For public-facing domains.
 -- Example: example.com → EC2 public IP.
- Private Hosted Zone
 -- Only resolvable within one or more VPCs.
 -- Example: db.internal → 10.0.1.5.
💲 Cost: $0.50/month per hosted zone.

9. **TTL (Time to Live)**
- TTL controls how long DNS resolvers cache your record.
- High TTL (e.g., 24 hrs)
✅ Less traffic to Route 53
❌ Slow changes propagation (old data may be served longer)
- Low TTL (e.g., 60 sec)
✅ Changes propagate faster
❌ More DNS queries (slightly higher cost)

**Example:**
- If TTL = 300s (5 minutes) and you change the IP, it can take up to 5 minutes for everyone to see the new IP.

10. **Real AWS Example**
- You have an EC2 instance for your app and want www.myapp.com to point to it:
-- Register domain in Route 53.
-- Create Public Hosted Zone for myapp.com.
-- Add A record:
--- www.myapp.com → 13.54.22.11 (EC2 Public IP), TTL 300.
- Test:
```ngnix
nslookup www.myapp.com
```






******************************


# 🟧 AWS Core Concepts – Quick Reference

This document provides a concise summary of AWS fundamental concepts including Regions, Availability Zones, Edge Locations, and IAM (Identity and Access Management).

---

## 🌍 AWS Regions

- A **Region** is a **cluster of data centers** located in a specific geographical area.
- **Data never leaves a region** without explicit permission.
- **Reduced latency** by deploying resources closer to customers.
- **Service availability** varies by region; not all features are available everywhere.
- **Pricing** differs between regions and is transparently published on the [AWS Pricing Page](https://aws.amazon.com/pricing/).

---

## 🏢 AWS Availability Zones (AZs)

- Each Region contains **multiple Availability Zones** (AZs).
- Each AZ is **one or more discrete data centers** with:
  - Redundant power
  - Independent networking
  - Separate cooling
- AZs are **isolated from failures/disasters**.
- AZs are **connected via high-bandwidth, ultra-low latency networking**.
- Recommended: **Deploy resources across AZs** for high availability and fault tolerance.

---

## 📍 AWS Points of Presence (Edge Locations)

- **Edge Locations** are used to deliver content to end users with **low latency**.
- They are part of AWS's **global infrastructure**.
- Utilized by **global services** such as:
  - **IAM**
  - **Amazon Route 53**
  - **Amazon CloudFront**
  - **AWS WAF**

---

## 🔐 AWS Identity and Access Management (IAM)

### IAM Basics

- **Global service**, not region-specific.
- A **root account** is created by default when you sign up for AWS.
- **Users** = people in your organization.
- **Groups** = collection of users (groups can't contain other groups).
- Users can:
  - Belong to multiple groups
  - Not belong to any group at all
- **IAM Roles**
  - An **IAM Role** is an identity with **permissions policies** that can be **assumed by trusted entities** (like users, applications, EC2 instances, AWS services, etc.).
  - Roles are **not associated with a specific user or group**
  - Instead, they are **assumed temporarily** to gain access to AWS resources.

  ✅ Key Use Cases of IAM Roles
  
  | Use Case                               | Example                                                                 |
  |----------------------------------------|-------------------------------------------------------------------------|
  | Grant AWS service permissions to resources | Allow an EC2 instance to access S3 using a role with S3 permissions     |
  | Cross-account access                   | Grant a user in Account A access to resources in Account B              |
  | Temporary credentials for users        | Allow federated users or apps to access AWS with temporary creds        |
  | Application or Lambda permissions      | Lambda assumes a role to access RDS, S3, DynamoDB, etc.                 |


### 🔑 IAM Permissions and Policies

- IAM uses **policies** (in JSON format) to define **permissions**.
- Follow the **Principle of Least Privilege**: only give the access that is needed.

#### Sample Policy Structure
```json
{
  "Version": "2012-10-17",
  "Id": "s3-acc-permissions",
  "Statement": [
    {
      "Sid": "ReadOnlyAccess",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::mybucket/*"
    }
  ]
}
```

**Fields Explanation:**
- `Version`: Always `"2012-10-17"` for IAM policies.
- `Id`: Optional policy identifier.
- `Statement`: Contains permissions.
  - `Sid`: Optional statement ID.
  - `Effect`: `Allow` or `Deny`.
  - `Principal`: The user/account/role the policy applies to.
  - `Action`: Actions allowed or denied.
  - `Resource`: AWS resources affected.
  - `Condition`: Optional logic for applying policy (e.g., IP-based).

---

### 🔒 IAM Password Policy & MFA

- Protect IAM and root users with strong **password policies**.
- Use **Multi-Factor Authentication (MFA)**:
  - Something you **know** (password) + Something you **have** (auth device).
  - Prevents account compromise even if password is stolen.

---

### 🛠️ Access Keys, AWS CLI, and SDK

| Tool              | Authentication                       |
|------------------|----------------------------------------|
| AWS Console       | Username + Password + MFA             |
| AWS CLI           | Access Key ID + Secret Access Key     |
| AWS SDKs          | Same as CLI, used in application code |

- **Access Keys**:
  - Created via IAM in AWS Console.
  - Managed by each IAM user.
  - Do not share access keys.
  - **Access Key ID** = Username
  - **Secret Access Key** = Password

---

✅ **Best Practices:**
- Always enable **MFA on root user**.
- Use **roles** for applications (e.g., EC2 instance profiles).
- Rotate **access keys** regularly.
- Review **IAM permissions** using AWS IAM Access Analyzer.

---
## 🔍 IAM Access Advisor & Credential Report

---

### ✅ IAM Access Advisor

**IAM Access Advisor** helps you analyze **what services a user or role has accessed** and **when**.  
It allows you to review **unused permissions** and follow the **least privilege principle**.

#### 📍 Where to Find:
- IAM Console → Users or Roles → Select a user/role → Access Advisor tab

#### 🧾 Access Advisor Output Example:

| AWS Service | Last Accessed     | Actions Granted       |
|-------------|-------------------|------------------------|
| S3          | 2025-07-12        | s3:ListBucket          |
| EC2         | Never Accessed    | ec2:StartInstances     |

Use this to **identify unused services** and **remove unnecessary permissions**.

---

### ✅ IAM Credential Report

The **Credential Report** is a CSV file that provides an overview of **all IAM users** and their **credential usage**.

#### 📍 Where to Find:
- IAM Console → Credential Report (left panel) → Click "Download Report"

#### 🧾 Credential Report Example:

| User | Password Enabled | Password Last Used | Access Key 1 Active | Access Key 1 Last Used | MFA Active |
|------|------------------|---------------------|----------------------|------------------------|------------|
| root | true             | 2025-07-10          | false                | -                      | true       |
| dev1 | true             | N/A                 | true                 | 2025-06-15             | false      |
| ops  | false            | -                   | true                 | Never                  | true       |

#### 🔍 Key Fields Explained:
- `password_enabled`: Is console password enabled?
- `password_last_used`: When was it last used?
- `mfa_active`: Is MFA enabled?
- `access_key_X_active`: Is access key active?
- `access_key_X_last_used`: When the access key was last used?

---

### 🧠 Use Cases

| Use Case                             | Tool              | Action                                  |
|--------------------------------------|-------------------|-----------------------------------------|
| Identify unused access keys          | Credential Report | Deactivate or delete keys               |
| Detect users without MFA             | Credential Report | Enable MFA or restrict access           |
| Remove unused permissions            | Access Advisor    | Apply least privilege principle         |
| Audit compliance and security        | Both              | Provide data for security reviews       |
| Detect inactive users                | Credential Report | Remove or disable IAM user accounts     |

---

### ✅ Best Practices:
- Review Access Advisor **monthly**.
- Enable **MFA** for all users.
- Rotate access keys **every 90 days**.
- Use **IAM roles** over users where possible.
- Regularly analyze the **Credential Report** for outdated or risky credentials.

