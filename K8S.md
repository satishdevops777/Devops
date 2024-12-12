## 1. Introduction to Kubernetes

Kubernetes is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications.

### Key Features:
- Automated Deployment and Scaling
- Self-Healing (auto-restarts, reschedules)
- Horizontal Scaling
- Service Discovery & Load Balancing

Example:
```bash
kubectl version --short
```
## 2. Kubernetes Architecture
Kubernetes follows a master-slave architecture.

### Components:
**Master Node:** Controls and manages the cluster. Contains the API server, scheduler, controller manager, etc.

**Worker Node:** Runs the application containers and includes components like Kubelet and Kube Proxy.

```
kubectl get nodes
```

## 3. Pods

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

## 4. ReplicaSets

A ReplicaSet ensures that a specified number of pod replicas are running at all times. It is often used in conjunction with Deployments.

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

## 5. Deployments

A Deployment provides declarative updates for Pods and ReplicaSets.

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
## 6. Namespaces

Namespaces allow you to divide cluster resources between multiple users via resource isolation.

Example: Creating a Namespace

```
kubectl create namespace dev
```
## 7. Services

A Service in Kubernetes is an abstraction that defines a logical set of Pods and enables network access to them.

Example: Service YAML

```
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
```

## 8. Volumes

Kubernetes Volumes are used for persistent storage that can be used by Pods.

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
## 9. ConfigMaps & Secrets

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

## 14. Ingress

Ingress is an API object that manages external access to services, typically HTTP.

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
