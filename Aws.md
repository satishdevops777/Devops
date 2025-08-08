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
Without cache: Every time your app needs data, it asks the database. This is slow if it happens millions of times. With cache: Your app checks the cache first. If the data is there (a cache hit), it returns instantly. If not (a cache miss), it goes to the database and stores the result in the cache for next time.

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
  App checks ElastiCache: GET product:1234

2. If cache hit: return fast

3. If cache miss:
  Get from database
  Store in cache: SET product:1234 {product_data} EX 3600 (cache for 1 hour)

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
  Frequently read data (e.g., product catalogs, user sessions, leaderboards)
  High-volume applications (gaming, social media, e-commerce)
  Serverless apps (like AWS Lambda) that need faster responses

🚫 Not ideal for:

Long-term data storage
Highly sensitive data that shouldn’t be cached
Cases where your data changes constantly every second

🔐 Security & Access
ElastiCache runs inside your VPC
You control access using security groups
Supports encryption, authentication, and IAM policies

🎯 Summary
Feature	Description
| Feature            | Description                                 |
| ------------------ | ------------------------------------------- |
| **What is it?**    | In-memory cache service                     |
| **Why use it?**    | Improve speed, reduce DB load, scale easily |
| **Types?**         | Redis (advanced), Memcached (simple)        |
| **Use cases?**     | Product info, user sessions, leaderboards   |
| **Fully managed?** | Yes – AWS handles setup, scaling, failover  |

🧠 Common Use Cases
Scenario A: Application using RDS + ElastiCache
```yaml
VPC: my-app-vpc
 ├─ Subnet 1: App Server (EC2 or Lambda)
 ├─ Subnet 2: RDS (MySQL/PostgreSQL)
 └─ Subnet 3: ElastiCache (Redis/Memcached)
```
✅ All components in the same VPC/subnets → they can talk over internal network → fast, secure, no extra config needed

✅ Recommended Setup
Use the same VPC and preferably the same subnet group or availability zone for:
  ElastiCache
  RDS
  Your application

This gives you:
  Low latency
  Secure private connections
  Simpler setup and maintenance



## Route53
1. What it is DNS?
Domain Name System (DNS) is the "phonebook of the internet."
It translates human-friendly hostnames into machine-readable IP addresses.
```scss
www.amazon.com  →  54.239.28.85
```
Without DNS, you’d have to type the IP address instead of the name.

2. Why it’s important:
Backbone of the Internet: Every web request starts with DNS resolution.
Hierarchical Naming Structure:
```scss
.com (Top-Level Domain)
     └── example.com (Second-Level Domain)
            └── www.example.com (Subdomain)
                   └── api.example.com (Nested Subdomain)
```
3. DNS Terminologies.
   
| Term                          | Meaning                                                               | Example                                           |
| ----------------------------- | --------------------------------------------------------------------- | ------------------------------------------------- |
| **Domain Registrar**          | Company that lets you register domain names.                          | Amazon Route 53, GoDaddy                          |
| **DNS Records**               | Mappings from names to IPs or other resources.                        | A, AAAA, CNAME, NS                                |
| **Zone File**                 | The file that stores DNS records for a domain.                        | Contains all `A`, `CNAME`, etc. for `example.com` |
| **Name Server**               | Server that answers DNS queries (Authoritative or Non-Authoritative). | ns-2048.awsdns-64.com                             |
| **TLD (Top-Level Domain)**    | Last part of the domain name.                                         | `.com`, `.in`, `.org`                            |
| **SLD (Second-Level Domain)** | Domain name under a TLD.                                              | `amazon.com`, `google.com`         |


4. How DNS Works – Step-by-Step
Let’s say you want to visit www.example.com:
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

5. Route 53 Overview
Amazon Route 53 is:

Highly available & scalable

Fully managed authoritative DNS

Lets you update your own DNS records

Can check health of your endpoints

The only AWS service with 100% SLA uptime guarantee


6. Route 53 Records
Each DNS record defines how you route traffic for a domain.

A record contains:

Name: example.com or app.example.com

Type: A, AAAA, CNAME, etc.

Value: IP or another hostname

Routing Policy: How Route 53 decides which record to use

TTL: How long the resolver caches the record

7. Record Types

| Record Type | Purpose                                              | Example                              |
| ----------- | ---------------------------------------------------- | ------------------------------------ |
| **A**       | Hostname → IPv4 address                              | `www.example.com → 192.0.2.44`       |
| **AAAA**    | Hostname → IPv6 address                              | `api.example.com → 2001:db8::1234`   |
| **CNAME**   | Hostname → another hostname (must resolve to A/AAAA) | `blog.example.com → www.example.com` |
| **NS**      | Nameservers for the hosted zone                      | `ns-2048.awsdns-64.com`              |

💡 AWS SAA Tip:
Route 53 also supports Alias records (AWS-specific) → Like CNAME but can be used for root domains and integrates with AWS resources like S3, CloudFront, ALB.

8. Hosted Zones
A Hosted Zone is a container for all the records for a domain.

Two types:

Public Hosted Zone

For public-facing domains.

Example: example.com → EC2 public IP.

Private Hosted Zone

Only resolvable within one or more VPCs.

Example: db.internal → 10.0.1.5.

💲 Cost: $0.50/month per hosted zone.

9. TTL (Time to Live)
TTL controls how long DNS resolvers cache your record.

High TTL (e.g., 24 hrs)
✅ Less traffic to Route 53
❌ Slow changes propagation (old data may be served longer)

Low TTL (e.g., 60 sec)
✅ Changes propagate faster
❌ More DNS queries (slightly higher cost)

Example:
If TTL = 300s (5 minutes) and you change the IP, it can take up to 5 minutes for everyone to see the new IP.

10. Real AWS Example
You have an EC2 instance for your app and want www.myapp.com to point to it:

Register domain in Route 53.

Create Public Hosted Zone for myapp.com.

Add A record:
www.myapp.com → 13.54.22.11 (EC2 Public IP), TTL 300.

Test:
```ngnix
nslookup www.myapp.com
```
