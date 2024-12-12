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

## Prometheus for monitoring

Fluentd for logging
Jaeger for tracing
Example: Deploying Prometheus
```
helm install prometheus prometheus-community/kube-prometheus-stack
```
