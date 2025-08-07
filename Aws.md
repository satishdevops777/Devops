## RDS Proxy

Amazon RDS Proxy is a fully managed database proxy service for Amazon RDS and Amazon Aurora. It helps improve application scalability, resiliency, and security by sitting between your application and your RDS/Aurora databases.
RDS Proxy is never publically accessible(Must be accessed from VPC)
**What is RDS Proxy?**
Imagine your application opens and closes many database connections rapidly. This can:
  1. Exhaust your DB instance resources (especially under heavy load).
  2. Cause performance bottlenecks.

**Amazon RDS Proxy helps by:**
  1. Pooling and reusing existing database connections.
  2. Managing connection limits.
  3. Automatically handling failovers in multi-AZ deployments.

Your app connects to RDS Proxy instead of directly to the RDS database.
```javascript
App ↔ RDS Proxy ↔ RDS/Aurora DB
```
  1. RDS Proxy keeps a pool of open connections to the database.
  2. Your app’s connections are routed through the proxy.
  3. Connections are reused efficiently.
  4. During failovers, the proxy maintains app connections while switching to the new DB node.

✅ Benefits of RDS Proxy
  | Feature                | Benefit                                 |
  | ---------------------- | --------------------------------------- |
  | **Connection pooling** | Reduces DB connection overhead.         |
  | **High availability**  | Seamless failover support.              |
  | **IAM authentication** | Use AWS IAM for secure access.          |
  | **TLS support**        | Encrypts traffic between app and proxy. |
  | **Security**           | Integrates with AWS Secrets Manager.    |

📦 Supported Databases
  Amazon RDS for MySQL
  Amazon RDS for PostgreSQL
  Amazon RDS for MariaDB
  Amazon Aurora (MySQL-compatible and PostgreSQL-compatible)

🛠️ How to Set Up RDS Proxy (Console)
  1. Go to RDS Console → Proxies → Create proxy
  2. Choose the DB engine (MySQL/PostgreSQL).
  3. Select the RDS or Aurora DB instance or cluster.
  4. Choose a Secrets Manager secret for DB credentials.
  5. Enable IAM authentication (optional).
  6. Configure VPC, subnets, and security groups.
  7. Create the proxy.

📌 When NOT to Use RDS Proxy?
    1. If your app has long-lived connections and doesn’t create many new ones.
    2. If your DB supports high connection counts without degradation.
    3. For unsupported database engines (e.g., Oracle or SQL Server, as of now).

**Extra:**

💡 What is "Connection Pooling"?
Connection pooling is like having a reusable set of doors into your database, instead of building a new door every time someone wants to enter.

🖥️ In Programming Terms:
Every time your app wants to talk to a database, it needs to:

Open a connection (expensive and slow)
Use it to run a query
Close the connection

Doing this repeatedly is slow and inefficient, especially under high traffic.
So instead, we use a connection pool:
A "pool" of open connections is kept ready.
When the app needs a connection, it borrows one from the pool.
When done, it returns it to the pool — no need to close and reopen.
This is faster and uses fewer resources.

🔁 Without vs With Connection Pooling

| Step             | Without Pooling        | With Pooling             |
| ---------------- | ---------------------- | ------------------------ |
| New connection?  | Every time             | Reuse from pool          |
| Open/close cost? | High                   | Low                      |
| App performance  | Slower under load      | Faster, more scalable    |
| Database load    | Heavy connection churn | Efficient connection use |

🔌 Why It's Important
Databases (like MySQL or PostgreSQL) can only handle limited connections.
Opening and closing connections frequently is slow.
Under high traffic, apps can crash or time out without pooling.

Example Scenario
Use Case:
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
Benefit:
The Lambda function quickly gets a connection from the proxy.
No connection storms during high loads.
Automatic failover during DB maintenance or failure.

Code Example (Node.js + MySQL)
Here’s how your code changes when using RDS Proxy:

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
