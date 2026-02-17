# Architecture

## 🔷 1️⃣ DESIGN (Architecture Planning)
  - You are building a Banking Mobile App like:
    - State Bank of India
    - HDFC Bank
  - App Features:
    - Login
    - Check balance
    - Transfer money
    - Transaction history
    - Notifications
  - Goal:
    - Never go down ❌
    - Secure 🔐
    - Handle 1 lakh users at once 📈
    - Cost controlled 💰

## 🌍 User Flow
```
User → API → Load Balancer → App Servers → Cache → Database → Queue
```
- And everything must survive if one data center (AZ) fails.
---
## 🌐 1️⃣ Networking

### 🔹 VPC (Your Private Data Center in AWS)
- CIDR: 10.0.0.0/16
- Inside VPC we create:
  | Subnet Type          | Purpose       |
  | -------------------- | ------------- |
  | Public Subnet        | Load Balancer |
  | Private Subnet       | App Servers   |
  | Private DB Subnet    | Database      |
  | Private Cache Subnet | Redis         |
  | Private MQ Subnet    | RabbitMQ      |

### Important Networking Components

| Component        | Why Needed                                  |
| ---------------- | ------------------------------------------- |
| Internet Gateway | Allow public access                         |
| NAT Gateway      | Private servers access internet for updates |
| Route Tables     | Traffic control                             |
| Security Groups  | Firewall at instance level                  |
| NACL             | Subnet-level firewall                       |

## 2️⃣ Entry Layer – API Gateway
- Amazon API Gateway
- Why?
  - Authentication
  - Rate limiting
  - Protect from attack
  - Central API management
  - User hits:
    ```
    https://api.bank.com/login
    ```
  - API Gateway forwards to backend.

 ## 3️⃣ Load Balancer
 - AWS Application Load Balancer
  - Distributes traffic
  - Health checks
  - Multi-AZ

## 4️⃣ Application Servers
- EC2 in private subnet
- Auto Scaling Group
- Minimum: 2 servers
- Max: 20 servers
- If traffic increases → AWS automatically launches more.

- Attaching IGW to VPC does NOT automatically give internet access.
