## 1. Introduction to Kubernetes

Kubernetes is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications.

- Kubernetes has two main parts:
  - Control Plane 
  - Worker Nodes 

### 1️⃣ Control Plane (Cluster Brain)
  - The Control Plane makes decisions.
  - It runs these core components:

  ### 🔹 API Server (kube-apiserver)
  - What it does:
    - Entry point to Kubernetes.
    - All commands go through it.
      ```
      kubectl apply -f app.yaml
      ```
      You → API Server → Cluster
    - It validates and stores your request.


  ### 🔹 etcd (Cluster Database)
  - Key-value store
  - Stores all cluster state
  - Source of truth
  - Example:
    - If you deploy 3 replicas, etcd stores:
      ```
      replicas = 3
      ```
    - If etcd dies → cluster state is gone.

  ### 🔹 Scheduler (kube-scheduler)
  - Decides: “Which node should run this Pod?”
  - Example:
    - If Node1 has 2 CPUs free and Node2 has 1 CPU free,
    - Scheduler picks Node1 for a 2-CPU Pod.
    - It considers:
      - CPU
      - Memory
      - Taints/tolerations
      - Affinity rules
  
  ### 🔹 Controller Manager (kube-controller-manager)
  - Ensures desired state = actual state.
  - Example:
  - You say:
    ```
    replicas: 3
    ```
  - One Pod crashes.
  - Controller sees:
    ```
    Current = 2
    Desired = 3
    ```
  - It creates 1 new Pod automatically.
  - This is the self-healing mechanism.


## 2️⃣ Worker Nodes (Where Apps Run)

### 🔹 Kubelet
  - Agent on each node
  - Talks to API server
  - Ensures Pods are running
  - Example
    - API says: “Run nginx pod”
  - Kubelet:
    - Pulls image
    - Starts container
    - Monitors health
  

  ### 🔹 Kube-Proxy
  - Handles networking.
  - It:
    - Routes traffic
    - Implements Services
    - Manages iptables rules
    - Example: Service IP → forwards traffic to correct Pod.

  ### 🔹 Container Runtime
  - Runs containers.
  - Examples:
    - containerd
    - CRI-O
  - Docker (older setups)
  - It actually executes:
    ```
    docker run nginx (behind the scenes)
    ```

## 🔁 How Everything Works Together

```
kubectl apply -f nginx.yaml
```
- Step-by-step:
- API Server receives request
- etcd stores desired state
- Scheduler picks a node
- Kubelet starts container
- Service exposes it
- Controller ensures it stays running
- If Pod crashes:
  - Controller detects mismatch
  - Creates new Pod
  - Self-healing happens

### Key Features:
- Automated Deployment and Scaling
- Self-Healing (auto-restarts, reschedules)
- Horizontal Scaling
- Service Discovery & Load Balancing
Example:
```bash
kubectl version --short
```

## 1. Pods

A Pod is the smallest deployable unit in Kubernetes, which can contain one or more containers that share the same network namespace.

Example: Pod YAML Definition
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
    - name: nginx
      image: nginx
      ports:
        - containerPort: 80
```
Command to create a pod:
```
kubectl apply -f pod.yaml
```

## 2. ReplicaSets

- A ReplicaSet ensures that a specified number of pod replicas are running at all times. It is often used in conjunction with Deployments.
- If one Pod crashes → ReplicaSet automatically creates a new one.

Example: ReplicaSet YAML
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replicaset
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx
          ports:
            - containerPort: 80
```

| Feature               | ReplicaSet | Deployment      |
| --------------------- | ---------- | ------------    |
| Pod scaling           | ✅          | ✅            |
| Self-healing          | ✅          | ✅            |
| Rolling updates       | ❌          | ✅            |
| Rollback              | ❌          | ✅            |
| Version history       | ❌          | ✅            |
| Used directly in prod | ❌ (rare)   | ✅ (standard) |


| Pod               | ReplicaSet           |
| ----------------- | -------------------- |
| Single instance   | Manages many Pods    |
| No self-healing   | Self-healing         |
| Manual recreation | Automatic recreation |
| Not scalable      | Scalable             |

- Deployments create and manage ReplicaSets for you.
- Old ReplicaSet stays (for rollback) but scaled to 0.
- Pods getting deleted unexpectedly
Cause:
- Another ReplicaSet or Deployment using same labels
  ```
  kubectl get rs --show-labels
  ```
- 📌 Labels must be unique per workload

- “A ReplicaSet ensures a fixed number of identical Pods are always running. It provides self-healing and scaling but does not support rolling updates. In production, ReplicaSets are typically managed by Deployments rather than used directly.”


## 3. Deployments
- A Deployment is a higher-level controller that:
  - Manages ReplicaSets
  - Ensures the desired number of Pods are running
  - Handles rolling updates, rollbacks, and scaling
  - version history

***A ReplicaSet ensures the desired number of identical Pods are running at all times using label selectors. It operates via Kubernetes’ reconciliation loop. However, in production we typically use Deployments, which manage ReplicaSets and provide rolling updates and rollback capabilities.***

### Why Deployments exist
- Without Deployments:
- Updates cause downtime 
- No rollback 
- Manual Pod recreation
  
### Deployments give you:
- Zero-downtime updates 
- Version history 
- Easy rollback 
- Declarative state management 
Example: Deployment YAML
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx
```
- If you delete ReplicaSet:
```
kubectl delete rs nginx-rs # Pods are deleted too (unless you use --cascade=orphan).
```

- If two ReplicaSets use same label selector:
  - They may fight over Pods.
  - This causes unpredictable behavior.
  - Avoid overlapping selectors.

- When you update a Deployment:
  - New ReplicaSet is created
  - Old ReplicaSet is scaled down
  - New ReplicaSet is scaled up
  - So rolling update = replica set swapping.
  
## Deployment Strategies

### 1️⃣ RollingUpdate (default)
```
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxSurge: 1
    maxUnavailable: 0
```
- maxSurge → Extra Pods allowed during update
- maxUnavailable → How many Pods can go down at once

- Flow:
- 1 new Pod starts (total = 5)
- 1 old Pod stops (total = 4)
- Repeat
  - Users never lose capacity.

- New Pods come up first
- Old Pods go down gradually
- Zero downtime if configured properly
- 💡 Production standard
  

### 2️⃣ Blue-Green Deployment
- You run two full environments:
  - Blue = current production
  - Green = new version
 When ready:
- Switch Service to point to Green
- Instant cutover.

####🔹 Pros
- Instant rollback
- Full testing before release

#### 🔹 Cons
- Double resource cost
- Not gradual

- A Kubernetes Service routes traffic based on labels.
- It does NOT know about versions.
- It just selects Pods with matching labels.
```
# Blue Deployment
labels:
  app: myapp
  version: blue

# Green Deployment
labels:
  app: myapp
  version: green

# Service Initially Points to Blue
selector:
  app: myapp
  version: blue

# You change Service selector:
selector:
  app: myapp
  version: green

```
```
terminationGracePeriodSeconds: 30
```
- App has 30 seconds to finish active requests.
  - t = 0s   Pod gets SIGTERM
  - t = 1s   Load balancer stops sending traffic
  - t = 2s   App stops accepting new requests
  - t = 5s   In-flight requests still running
  - t = 30s  SIGKILL if app hasn’t exited
- Grace Period Too Short
  - In-flight requests need 60s
  - Grace period = 30s
  - 💥 At 30s → SIGKIL
  - 💥 Requests terminated → 5xx

***“Rolling updates usually avoid downtime because traffic is shifted gradually, while blue-green deployments can cause downtime if traffic is switched before the new version is fully ready. However, both can be zero-downtime with proper readiness checks and graceful shutdown.”***

### 3️⃣ Canary Deployment
- Release new version to small % of users first.
- Example:
  ```
  90% traffic → v1
  10% traffic → v2
  ```
- If stable → increase traffic gradually.

#### 🔹 How It's Done
- Options:
  - Separate Deployments with weighted Ingress
  - Service mesh (Istio, Linkerd)
  - NGINX ingress weight routing

#### 🔹 Best For
  - Risky releases
  - Large-scale systems
  - Production safety

### 4️⃣ Recreate
```
strategy:
  type: Recreate
```

- Old Pods killed first
- New Pods created next
- Causes downtime ❌
Used when:
- App cannot run multiple versions
- DB schema breaking changes
```
kubectl rollout history deployment payment-deploy
kubectl rollout undo deployment payment-deploy
kubectl rollout undo deployment payment-deploy --to-revision=2
kubectl scale deployment payment-deploy --replicas=5
```
- “A Deployment manages ReplicaSets and Pods declaratively, providing scaling, rolling updates, and rollbacks. It is the standard way to run stateless applications in Kubernetes and enables zero-downtime deployments.”
- “Rollback issues often happen when revisionHistoryLimit is set too low, because Kubernetes deletes old ReplicaSets required for rollback.”

- “Rollbacks can fail even when history exists because rollback is just another rollout. If configs, probes, schemas, or infrastructure have changed, the old version may no longer start or become ready. That’s why mature teams design for backward compatibility and test rollback paths.”
```
kubectl set image deployment payment-deploy \
payment=payment-service:v2

(This created a new ReplicaSet, not a rollback)
kubectl patch deployment payment-deploy \
-p '{"spec":{"revisionHistoryLimit":10}}'
```
- “When rollback fails even with an old ReplicaSet present, I inspect rollout status, Pod events, logs, probe failures, config drift, and resource constraints. If needed, I bypass rollback by scaling the old ReplicaSet or forcing an image rollback to create a fresh ReplicaSet, restoring service first and fixing root causes afterward.”



## 🔐 Critical Requirements for Zero Downtime

- Even with Rolling Update, you must configure:

### 1️⃣ Readiness Probes
- If not configured: Kubernetes may send traffic to unhealthy Pods.
  ```
  readinessProbe:
  httpGet:
    path: /health
    port: 8080
  initialDelaySeconds: 10 # Wait 10 seconds after the container starts before running the first readiness check.
  periodSeconds: 5   # Run the readiness probe every 5 seconds.
  timeoutSeconds: 2  # If /health does not respond within 2 seconds, that probe attempt is considered FAILED.
  failureThreshold: 3 # After 3 consecutive failed probes, mark the pod as NOT READY.
  ```
- Traffic only goes to Pods that pass readiness.

### 2️⃣ Proper Resource Requests
- If new Pods can’t schedule (insufficient CPU/memory):
- Deployment may stall.

### 3️⃣ Pod Disruption Budget (PDB)
- Prevents too many Pods from being down.
  ```
  minAvailable: 2
  ```
- Guarantees minimum availability.

### 4️⃣ Graceful Shutdown
```
terminationGracePeriodSeconds
```
- And handle SIGTERM in app.
- Prevents dropped requests.

***Kubernetes supports zero-downtime deployments primarily using rolling updates with maxUnavailable set to 0 and proper readiness probes. For higher safety, blue-green or canary strategies can be implemented using separate deployments and traffic shifting via ingress or service mesh.***

## 4. Namespaces

Namespaces allow you to divide cluster resources between multiple users via resource isolation.
- Organize resources
- Isolate environments
- Apply security policies
- Control resource usage

- Without namespaces:
 - All Pods, Services, Deployments live together
 - Name conflicts happen
 - Hard to apply team-level permissions
 - No resource isolation

- With namespaces:
 - You can have the same app name in different environments
 - Apply RBAC per team
 - Limit CPU/memory usage per namespace

Example: Creating a Namespace

```
kubectl create namespace dev
```

## 5. Services
- A stable network identity (IP + DNS) to access a group of Pods.
- Ephemeral (they die and restart)
- Their IP addresses change
- Service gives:
  - Stable IP
  - Load balancing
  - Service discovery
- 1️⃣ Service selects Pods using labels
- 2️⃣ Endpoints / EndpointSlices created: These are Pod IPs, updated dynamically.
- 3️⃣ kube-proxy routes traffic
  - Service gets a ClusterIP
  - kube-proxy programs iptables / IPVS
  - Traffic load-balanced at L4 (TCP/UDP)

### 1️⃣ ClusterIP (default)
- Internal only
- Used for microservice communication
- Most common
- Use case: Frontend → Backend communication

### 2️⃣ NodePort
- Exposes service on every node
- Opens port like 30000–32767
```
http://nodeIP:30007
nodeIP:nodePort
```
- Used for testing / debugging

⚠️ Not recommended for prod internet traffic.

### 3️⃣ LoadBalancer (cloud)
- Creates cloud LB (ELB, ALB, NLB, etc.)
- Public or private
- Production external access
- Example in AWS: Creates ELB/NLB automatically

### 4️⃣ Headless Service (IMPORTANT)
- No load balancing
- DNS returns Pod IPs directly
- Used by StatefulSets (DBs, Kafka)
  ```
  clusterIP: None
  ```
### 4️⃣ ExternalName
- Maps service to external DNS name.
Example:
```
db.external.com
```
- Used for external database integration.

| Field      | Meaning                           |
| ---------- | --------------------------------- |
| port       | Service port                      |
| targetPort | Container port                    |
| nodePort   | Exposed node port (NodePort only) |

Example: Service YAML

```
apiVersion: v1
kind: Service
metadata:
  name: backend
spec:
  selector:
    app: backend
  ports:
  - port: 80
    targetPort: 8080
```
Service:
- Listens on port 80
- Forwards to container port 8080
- Load balances across all Pods with app=backend

-  “targetPort is the port where the application listens inside the container. port is the Service’s virtual port used by clients. nodePort exposes that Service on every node’s IP for external access. Traffic flows from nodePort → port → targetPort.”
-  “A Service provides a stable virtual IP and DNS name to access a dynamic set of Pods selected via labels. It abstracts Pod IP changes and provides L4 load balancing via kube-proxy using iptables or IPVS.”
- 1️⃣ Service selector mismatch → No endpoints
- 2️⃣ Readiness failing → No traffic
- 3️⃣ Wrong targetPort → Connection refused
- 4️⃣ NodePort blocked by firewall
- 5️⃣ LoadBalancer provisioning delay

| Scenario        | Port to use |
| --------------- | ----------- |
| Pod → Pod       | targetPort  |
| Pod → Service   | port        |
| External → Node | nodePort    |

## How kube-proxy Works Internally
---
- kube-proxy is a small program that runs on every node.
- Make Services send traffic to the correct Pods.
- kube-proxy:
- Randomly picks one Pod (iptables mode)
- Or uses round-robin (IPVS mode)
- That’s how load balancing happens.

### What If kube-proxy Stops?
- New Services won’t work
- Pod changes won’t update routing
- Eventually networking breaks.
***kube-proxy runs on each node and programs Linux networking rules so that traffic sent to a Service IP is automatically redirected to one of the backend Pod IPs.***

- Pods can talk across nodes because:
- CNI creates: Overlay network (like virtual tunnel) OR Direct routing rules
- So cross-node communication is handled by CNI, not kube-proxy.


## 6. Ingress

- Ingress is an API object that manages external access to services, typically HTTP.
- A Layer 7 (HTTP/HTTPS) traffic router inside Kubernetes.
- It allows:
  - Host-based routing
  - Path-based routing
  - TLS termination
  - SSL certificates
  - Rewrite rules

Example: Ingress YAML

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nginx-ingress
spec:
  rules:
    - host: nginx.local
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: nginx-service
                port:
                  number: 80
```
- Service → Endpoints → Labels → Readiness → Internal Test → kube-proxy → NetworkPolicy → External LB


## 7. Volumes

- A Volume in Kubernetes is a way to store and share data that:
- Survives container restarts
- Can be shared between containers in a Pod
- Can be ephemeral or persistent
- Kubernetes Volumes are used for persistent storage that can be used by Pods.
- A Kubernetes volume provides storage to Pods that is independent of the container lifecycle.

### Why volumes exist (real problem)

- Without volumes:
  - Container restarts = data lost 
  - Sidecar containers can’t share files 
  - Logs/config/state disappear 

- Volumes solve:
  - Persistence
  - Sharing
  - Decoupling storage from containers

```
Application
   ↓
Pod
   ↓
Volume
   ↓
PVC
   ↓
PV
   ↓
Storage (EBS / NFS / Disk / Cloud)
```

### Volume types

### 1️⃣ emptyDir (ephemeral)
- Created when Pod starts
- Deleted when Pod dies
- Shared across containers in same Pod
  ```
  volumes:
  - name: cache-volume
    emptyDir: {}
  ```
### Use cases:
- Temp files
- Caches
- Sidecar log sharing

### 2️⃣ hostPath (dangerous in prod)
- Mounts node filesystem into Pod
- Ties Pod to a node
- Use cases:
  - Node-level agents
  - Debugging

### 3️⃣ ConfigMap & Secret (special volumes)
- Inject config files
- Inject secrets as files
  ```
  volumes:
  - name: config
    configMap:
      name: app-config
  ```
 🚫 Avoid for apps.
 
### 4️⃣ Persistent Volume (PV) & PVC (MOST IMPORTANT)
### PersistentVolume (PV)
- Cluster-level storage
- Backed by disk (EBS, NFS, etc.)

### PersistentVolumeClaim (PVC)
- Storage request by Pod
- Binds to a PV


- Pod requests storage via PVC. -- “I need 10GB storage.”
- Instead of manually creating PV: Use StorageClass.
- It dynamically provisions storage.
- Example:
  - In AWS → Automatically creates EBS volume
  - In GCP → Creates Persistent Disk
- PVC requests → StorageClass creates PV automatically.

```
Pod → PVC → StorageClass → EBS created → Attached to Node → Mounted in Pod
```
- If Pod moves to another node: Volume detaches & Reattaches to new node

### Common Production Issues
- 1️⃣ PVC Pending (no matching PV) - → No StorageClass or CSI issue
- 2️⃣ Wrong access mode
- 3️⃣ Node can’t attach disk - → Node not ready
- 4️⃣ EBS stuck in attaching state - → IAM permission issue
- 5️⃣ File system permissions wrong
- 6️⃣ Disk full
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: data-pvc
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
```
```
volumes:
- name: data
  persistentVolumeClaim:
    claimName: data-pvc
```

| Mode | Meaning             |
| ---- | ------------------- |
| RWO  | Mounted by one node |
| ROX  | Read-only by many   |
| RWX  | Read-write by many  |

💡 EBS = RWO
💡 NFS/EFS = RWX

### StatefulSets + Volumes (production pattern)
- Each Pod gets its own PVC
- PVC survives Pod restart
- Used for DBs, Kafka, Redis

```
Application (Pod)
   ↓ asks for storage
PVC (request)
   ↓ binds to
PV (actual storage)
   ↓ provisioned by
StorageClass (how storage is created)
```

Example: Persistent Volume (PV) and Persistent Volume Claim (PVC)
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: nginx-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /tmp/data
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: nginx-pvc
spec:
  resources:
    requests:
      storage: 1Gi
```

- “PV = the disk that exists somewhere”
- 1️⃣ StorageClass = How storage is created
✔ Defines:
- Which storage (EBS, EFS, NFS, Ceph, etc.)
- How it’s created (disk type, IOPS, AZ behavior)
- Which CSI driver
```
provisioner: ebs.csi.aws.com   # EBS
provisioner: efs.csi.aws.com   # EFS

```
- “StorageClass defines how storage like EBS or EFS is provisioned, a PVC requests how much storage is needed, Kubernetes creates or binds a PV, and the Pod mounts the PVC.”
- - “StorageClass defines how storage is provisioned, PV represents the actual storage, PVC is a request for storage, and Pods mount PVCs—not PVs directly.”

## 8. ConfigMaps & Secrets

ConfigMap: Stores configuration data in key-value pairs.
Secrets: Stores sensitive information such as passwords or API keys.

Example: ConfigMap YAML

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_ENV: "production"
  APP_DEBUG: "false"
Example: Secret YAML
yaml
Copy code
apiVersion: v1
kind: Secret
metadata:
  name: db-password
type: Opaque
data:
  password: cGFzc3dvcmQ=  # base64 encoded password
```
## 10. StatefulSets

A StatefulSet is used to manage stateful applications.
```
Example: StatefulSet YAML
yaml
Copy code
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: redis
spec:
  serviceName: "redis"
  replicas: 3
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
        - name: redis
          image: redis
          ports:
            - containerPort: 6379
```
## 11. DaemonSets

A DaemonSet ensures that all (or some) nodes run a copy of a pod.

Example: DaemonSet YAML

```
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd
spec:
  selector:
    matchLabels:
      name: fluentd
  template:
    metadata:
      labels:
        name: fluentd
    spec:
      containers:
        - name: fluentd
          image: fluent/fluentd
```
## 12. Jobs & CronJobs

Job: A Job creates one or more Pods and ensures that a specified number of them successfully terminate.
CronJob: A CronJob creates Jobs on a scheduled basis.

Example: Job YAML

```
apiVersion: batch/v1
kind: Job
metadata:
  name: pi
spec:
  template:
    spec:
      containers:
        - name: pi
          image: perl
          command: ["perl", "-Mbignum=bpi", "-wle", "print bpi(2000)"]
      restartPolicy: Never
```
Example: CronJob YAML

```
apiVersion: batch/v1
kind: CronJob
metadata:
  name: pi-cronjob
spec:
  schedule: "*/5 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name: pi
              image: perl
              command: ["perl", "-Mbignum=bpi", "-wle", "print bpi(2000)"]
          restartPolicy: OnFailure
```
## 13. Horizontal Pod Autoscaling

Horizontal Pod Autoscaler (HPA) automatically scales the number of Pods based on observed CPU utilization or other select metrics.

Example: HPA YAML

```
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: nginx-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nginx-deployment
  minReplicas: 1
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 50
```




## 15. Network Policies
    
Network Policies allow you to control the communication between Pods and Services.

Example: Network Policy YAML

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-frontend
spec:
  podSelector:
    matchLabels:
      app: frontend
  ingress:
    - from:
        - podSelector:
            matchLabels:
              app: backend
      ports:
        - protocol: TCP
          port: 80
```
## 16. Helm

Helm is a package manager for Kubernetes that allows you to define, install, and manage Kubernetes applications.

Example: Installing Helm Chart

```
helm install my-release bitnami/nginx
```

## 17. Operators and Custom Resources

An Operator is a method of packaging, deploying, and managing a Kubernetes application. Operators use Custom Resource Definitions (CRDs) to extend the Kubernetes API.

Example: Custom Resource Definition (CRD) YAML

```
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: mysqlclusters.mysql.example.com
spec:
  group: mysql.example.com
  names:
    kind: MysqlCluster
    plural: mysqlclusters
    singular: mysqlcluster
  scope: Namespaced
  versions:
    - name: v1
      served: true
      storage: true
      schema:
        openAPIV3Schema:
          type: object
          properties:
            spec:
              type: object
              properties:
                size:
                  type: integer
                  description: "Number of replicas"
```
## 18. Kubernetes Security
Kubernetes security involves various strategies like RBAC (Role-Based Access Control), network policies, and secrets management.

Example: RBAC YAML

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: default-view
subjects:
  - kind: User
    name: "default"
    apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: view
  apiGroup: rbac.authorization.k8s.io
```
## 19. Multi-Cluster Management

Multi-cluster management allows you to manage several Kubernetes clusters from a central point. Tools like Rancher and KubeFed provide multi-cluster orchestration.

## 20. Kubernetes Observability

Observability includes monitoring, logging, and tracing to provide insights into the health and performance of applications.


## 21. Kubernetes Cluster Autoscaler

The Cluster Autoscaler is a Kubernetes component that automatically adjusts the size of a cluster by adding or removing nodes based on resource utilization. It helps maintain optimal performance while minimizing cost by ensuring that the cluster has enough resources for Pods to run without over-provisioning the infrastructure.

The Cluster Autoscaler works with cloud providers (AWS, GCP, Azure, etc.) to scale the underlying infrastructure based on the needs of the Kubernetes workloads.

### Key Concepts

Node Group: A group of nodes with similar characteristics (e.g., instance type, region). The Cluster Autoscaler interacts with the cloud provider's API to scale the number of nodes in these groups.

Scaling Up: The Cluster Autoscaler adds nodes to the cluster when there aren't enough resources to schedule pending Pods.

Scaling Down: The Cluster Autoscaler removes nodes when they are underutilized, and their Pods can be rescheduled on other nodes.

***Prerequisites***

A Kubernetes cluster running on a supported cloud provider (e.g., AWS, GCP, Azure).

IAM permissions for the Cluster Autoscaler to manage the cluster's node group.

***Installation***

### 1. Installing Cluster Autoscaler on Kubernetes

For example, to deploy Cluster Autoscaler on AWS EKS:

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/autoscaler/master/cluster-autoscaler/cloudprovider/aws/examples/cluster-autoscaler.yaml
```
Make sure to edit the cluster-autoscaler.yaml to include your AWS region and other configurations (e.g., specifying the correct IAM role for the autoscaler).

### 2. Configure RBAC for Cluster Autoscaler
RBAC permissions are required for Cluster Autoscaler to interact with the Kubernetes API.

Example ClusterRole and ClusterRoleBinding:
```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRole
  metadata:
    # Add a role for Cluster Autoscaler to manage resources
    name: cluster-autoscaler
rules:
  - apiGroups: [""]
    resources: ["nodes", "pods"]
    verbs: ["list", "watch", "get", "update"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: cluster-autoscaler-binding
subjects:
  - kind: ServiceAccount
    name: cluster-autoscaler
    namespace: kube-system
roleRef:
  kind: ClusterRole
  name: cluster-autoscaler
  apiGroup: rbac.authorization.k8s.io
```
### 3. Configure Cloud Provider for Scaling

You must set up your cloud provider to allow Kubernetes to scale resources. For example, on AWS:

IAM Role: Ensure that the IAM role assigned to the Cluster Autoscaler has the necessary permissions to scale EC2 instances.

Auto Scaling Group: Set up an Auto Scaling Group (ASG) for your worker nodes to allow the autoscaler to adjust node counts.

Example Configuration of Cluster Autoscaler

Here’s an example configuration to deploy the Cluster Autoscaler in a Kubernetes cluster (AWS as the cloud provider):

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: cluster-autoscaler
  namespace: kube-system
spec:
  replicas: 1
  selector:
    matchLabels:
      app: cluster-autoscaler
  template:
    metadata:
      labels:
        app: cluster-autoscaler
    spec:
      containers:
        - name: cluster-autoscaler
          image: k8s.gcr.io/cluster-autoscaler:v1.24.0
          command:
            - ./cluster-autoscaler
            - --v=4
            - --cloud-provider=aws
            - --nodes=1:10:your-node-group-name
            - --scale-down-enabled=true
            - --scale-down-delay-after-add=10m
            - --scale-down-unneeded-time=10m
            - --skip-nodes-with-local-storage=false
            - --expander=least-waste
            - --balance-similar-node-groups=true
            - --use-service-account-credentials=true
          env:
            - name: CLUSTER_NAME
              value: "your-cluster-name"
          volumeMounts:
            - mountPath: /etc/ssl/certs/ca-certificates.crt
              name: ssl-certs
              readOnly: true
      volumes:
        - name: ssl-certs
          hostPath:
            path: /etc/ssl/certs/ca-certificates.crt
            type: File
```
This configuration: Deploys the Cluster Autoscaler with a specific version (v1.24.0). Configures scaling limits for the node group (from 1 to 10 nodes).
Enables scale-down and sets parameters like scale-down-delay-after-add and scale-down-unneeded-time.

### Scaling Options

Scale-Up: The autoscaler will increase the number of nodes if there are unschedulable pods.

Scale-Down: The autoscaler will decrease the number of nodes if a node is underutilized and its Pods can be rescheduled to other nodes.

You can customize scaling parameters like:

--scale-down-delay-after-add: The time after a node is added before scaling down is triggered.

--scale-down-unneeded-time: The time a node should be underutilized before it is scaled down.

--expander: Defines the strategy to use for scaling decisions (e.g., least-waste, most-pods, price).

Scaling Example

Scenario 1: If a Pod is pending due to insufficient resources (CPU or memory), the Cluster Autoscaler will try to add a new node to accommodate the Pod.

Scenario 2: If a node is underutilized and Pods can be moved, the Cluster Autoscaler will remove the node to save resources.

Troubleshooting

Check Cluster Autoscaler logs: To inspect the Cluster Autoscaler logs:

```
kubectl -n kube-system logs deployment/cluster-autoscaler
```
Verify Node Group Configuration: Ensure your cloud provider's Auto Scaling Group is properly configured to support automatic scaling.

## Prometheus for monitoring

Fluentd for logging
Jaeger for tracing
Example: Deploying Prometheus
```
helm install prometheus prometheus-community/kube-prometheus-stack
```
***A finalizer is a metadata field on a Kubernetes resource that blocks deletion until a controller performs necessary cleanup operations. It ensures external dependencies such as cloud load balancers or persistent volumes are properly cleaned before the resource is fully removed.***


## Taints & Tolerations
- A taint is applied to a node.
```
key=value:effect
```

- Common effects
  - NoSchedule → new pods won’t be scheduled
  - PreferNoSchedule → avoid if possible
  - NoExecute → evict running pods
  ```
  kubectl taint node node1 infra=true:NoSchedule # Node says: “Only infra pods are allowed here.”
  ```

- A toleration is applied to a pod.
  ```
  tolerations:
  - key: "infra"
    operator: "Equal"
    value: "true"
    effect: "NoSchedule"
  ```

- Node affinity attracts pods to nodes based on labels.
