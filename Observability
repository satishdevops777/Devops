# OBSERVABILITY
- Observability is the capability of a system to explain its internal state using telemetry data like logs, metrics, and traces.

👉 If your system breaks at 3 AM, observability tells you what broke, where, why, and what to do next.

## Why Observability Exists
### Traditional monitoring answers:
- ❓ Is the system up?
- ❓ Is CPU high?
### Observability answers:
- ✅ Which service is slow?
- ✅ Which user request failed?
- ✅ What change caused the outage?
- ✅ Is this infra, app, or dependency related?

## The Three Pillars of Observability 🧱
### 1️⃣ Metrics (What is happening?)
- Numeric values over time
- Fast, cheap, good for alerts
### Examples
- CPU usage
- Memory usage
- Request rate (RPS)
- Error rate
- Latency (P95, P99)
### Tools
- Prometheus
- Grafana
- CloudWatch
- SignalFx

### 2️⃣ Logs (Why did it happen?)

<img width="868" height="511" alt="image" src="https://github.com/user-attachments/assets/d599ac15-f8c3-4b6e-98c5-a83a794c8c7e" />

- Detailed, contextual records
- Best for debugging
```ERROR payment-service timeout while calling bank-api```
### Tools
- Splunk
- ELK / OpenSearch
- CloudWatch Logs

### 3️⃣ Traces (Where did it happen?)
- Follow a request across services
- Shows latency & bottlenecks
- User → API Gateway → Auth → Order → Payment → DB

### Tools
- Jaeger
- Zipkin
- OpenTelemetry
- X-Ray

```“Observability allows us to understand system behaviour and diagnose unknown failures using metrics, logs, and traces, reducing MTTR and improving reliability.”```

## Splunk and SignalFx
- Splunk = logs (deep details, forensics)
- Signal = metrics + monitoring (real-time health, alerts)
### Together, they answer:
- Is something wrong? → SignalFx
- Where is it wrong? → SignalFx
- Why did it happen? → Splunk
 <img width="673" height="217" alt="image" src="https://github.com/user-attachments/assets/3deaa414-f7a8-4c21-a444-c53312b02fc4" />


### Components You’re Using
### Splunk
- Centralized log storage & search
- Root cause analysis
- Incident forensics
### SignalFx
- Metrics, dashboards, alerts
- Real-time monitoring
- APM & infrastructure visibility (Now part of Splunk Observability Cloud)

## 1️⃣ Monitoring with SignalFx (Metrics-Driven)
### What you monitor in SignalFx
### 🔹 Infrastructure Metrics
- CPU, memory, disk, network
- Node health
- Load average

### 🔹 Kubernetes Metrics
- Pod restarts
- Pod CPU/memory throttling
- Node pressure
- Deployment replica health

### Application Metrics (Golden Signals ⭐)
- Latency (P95 / P99)
- Traffic (RPS)
- Errors (4xx / 5xx)
- Saturation

### How data gets into SignalFx
- SignalFx Smart Agent / OpenTelemetry
- K8s metrics-server
- Cloud integrations (AWS, Azure, GCP)
- App instrumentation (Java, Node, Python, etc.)

### Example Alert in SignalFx 🚨
 <img width="542" height="166" alt="image" src="https://github.com/user-attachments/assets/da39c50a-574e-44cd-9bdf-0b1b5f7f1c88" />

👉 This tells you something is wrong
- But not yet why.
- 
## 2️⃣ Logging & Deep Debugging with Splunk
### What goes into Splunk
- Application logs
- Kubernetes pod logs
- Node/system logs
- API gateway logs
- Error & exception traces


### Example Logs in Splunk 🔍
<img width="455" height="128" alt="image" src="https://github.com/user-attachments/assets/d0d17cb3-fcb8-4c8b-9ad4-b04e18303978" />

### Now you know:
- Which service
- Which dependency
- Exact failure reason

## 3️⃣ How Monitoring + Observability Work Together
### 🚨 Incident Flow (Real World)
- Step 1: Alert fires in SignalFx
- High latency in payment-service

### Step 2: Open SignalFx dashboard
You see:
- CPU normal ❌
- Memory normal ❌
- Latency high ✅
- Error rate rising ✅
👉 Problem is application or dependency, not infra

### Step 3: Pivot to Splunk (Logs)
- service=payment-service environment=prod ERROR
- DB connection pool exhausted

### Step 4: Root Cause
- Traffic spike
- DB max connections reached
- App retries increased latency

### Step 5: Fix
- Scale DB
- Increase pool
- Add rate limit / caching
- MTTR drops drastically

## 4️⃣ Correlation (This Is Observability 🔥)

| SignalFx     | Splunk          |
| ------------ | --------------- |
| Alert        | Error logs      |
| Metric spike | Stack trace     |
| Dashboard    | Request context |
| SLO breach   | Root cause      |

💡 Same service name, host, pod, request ID

## 5️⃣ SLOs with SignalFx + Splunk
### SignalFx
- Tracks SLI metrics (latency, error rate)
- Calculates SLO compliance
- Alerts on burn rate

### Splunk
- Explains why SLO was violated
- Provides evidence for RCA

## 6️⃣ How You’d Explain This in an Interview 🎤
- “We use SignalFx for real-time monitoring and alerting based on metrics like latency, error rate, and saturation. When an alert fires, we correlate it with logs stored in Splunk to perform root cause analysis. This combination allows us to detect issues early, reduce MTTR, and handle both known and unknown failure scenarios effectively.”


## 7️⃣ Summary (One Screen 🧠)

### ✅ SignalFx
- Monitoring
- Dashboards
- Alerts
- SLOs

### ✅ Splunk
- Logs
- Debugging
- RCA
- Audit & forensics

### ✅ Togethe = True Observability

## 1️⃣ Latency Percentiles: P95 & P99 (the “slow users” story)
- Latency = time taken to serve a request
- Example: /checkout API took 320 ms

### What does P95 mean?
- P95 latency = 95% of requests are faster than this value
- The slowest 5% are slower than this

### What does P99 mean?
- P99 latency = 99% of requests are faster than this value
- The slowest 1% are slower than this

## 2️⃣ Tracing (following a single request 🔍)
- Metrics say “something is slow
- Tracing says “THIS is where it’s slow”

### What is a Trace?
- A trace follows one request as it travels through multiple services.

### Example Trace Scenario
- User clicks “Place Order”
```
  User
 ↓ 50ms
API Gateway
 ↓ 80ms
Auth Service
 ↓ 900ms  ❌
Payment Service
 ↓ 120ms
Database
```
🧠 From this trace:
- Total latency = ~1.15s
- Payment Service is the bottleneck
- Now you know where to look

### How tracing + P99 work together
- P99 alert fires → “Some users are very slow”
- You open a trace from a slow request
- You see exact service & dependency causing delay

## 3️⃣ Error Rate (how often things fail ❌)
### What is error rate?
- Error rate = percentage of failed requests
```
Error Rate = (Failed Requests / Total Requests) × 100
```
### What counts as an error?
- Usually:
 - HTTP 5xx → server errors
 - Sometimes 4xx (depends on design)
 - Timeouts
 - Dependency failures

## 4️⃣ SLIs (Service Level Indicators)
- SLI = what you measure
- These are raw metrics.
### Common SLIs (Golden Signals ⭐)

| SLI Type     | Example               |
| ------------ | --------------------- |
| Latency      | P95 < 500 ms          |
| Availability | % successful requests |
| Error rate   | < 1%                  |
| Throughput   | Requests per second   |
| Saturation   | CPU / memory usage    |

```SLI: Percentage of requests completed under 500 ms```

## 5️⃣ SLOs (Service Level Objectives)
- SLO = the target you promise internally
- Built on top of SLIs.
```
99.9% of checkout requests
must complete under 500 ms
over a rolling 30 days
```
- 0.1% requests can be slow
- Error budget exists

## 6️⃣ SLAs (Service Level Agreements)
- SLA = legal / customer-facing promise
- If you break it → 💸 penalties


## How We get Metrics, Traces and Logs?

```
Application
 ├─ Metrics + Traces → OpenTelemetry SDK
 │                     ↓
 │              OTel Collector
 │                     ↓
 │                SignalFx
 │
 └─ Logs → Fluent Bit / Splunk Forwarder → Splunk
````

### 1️⃣ Deploy SignalFx OpenTelemetry Collector on Servers / VMs

### When you do this
- Legacy apps
- EC2 / VM workloads
- Non-Kubernetes services

### Step 1: Download the Collector

```
curl -LO https://github.com/signalfx/splunk-otel-collector/releases/latest/download/splunk-otel-collector_linux_amd64.deb
sudo dpkg -i splunk-otel-collector_linux_amd64.deb
```

### Step 2: Configure the Collector
### File: /etc/otel/collector/agent_config.yaml

```yaml
receivers:
  otlp:
    protocols:
      grpc:
      http:

processors:
  batch:
  resource:
    attributes:
      - key: service.environment
        value: prod
        action: upsert

exporters:
  signalfx:
    access_token: ${SPLUNK_ACCESS_TOKEN}
    realm: us0   # or eu0, etc.

service:
  pipelines:
    metrics:
      receivers: [otlp]
      processors: [resource, batch]
      exporters: [signalfx]

    traces:
      receivers: [otlp]
      processors: [resource, batch]
      exporters: [signalfx]
```

### Step 3: Start the service
```
sudo systemctl enable splunk-otel-collector
sudo systemctl start splunk-otel-collector
```
✅ Your VM can now receive metrics & traces.


### 2️⃣ Deploy SignalFx OpenTelemetry Collector on Kubernetes
- Best practice
 - DaemonSet → node-level metrics
 - Deployment → app-level telemetry aggregation

```
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: otel-collector
spec:
  selector:
    matchLabels:
      app: otel-collector
  template:
    metadata:
      labels:
        app: otel-collector
    spec:
      containers:
      - name: otel-collector
        image: quay.io/signalfx/splunk-otel-collector:latest
        env:
        - name: SPLUNK_ACCESS_TOKEN
          valueFrom:
            secretKeyRef:
              name: splunk-secret
              key: token
        - name: SPLUNK_REALM
          value: us0
```

### 3️⃣ How we instrument applications (THIS IS KEY)
- Metrics don’t magically appear.
- Instrumentation = code + runtime hooks

- Java application (most common)
 - Step 1: Add Java agent (no code change!)
```
java -javaagent:/otel/opentelemetry-javaagent.jar \
     -Dotel.service.name=payment-service \
     -Dotel.exporter.otlp.endpoint=http://otel-collector:4317 \
     -jar app.jar
```
### What you get automatically:
- HTTP latency
- DB calls
- Error counts
- Full traces

### What telemetry is generated

| Signal | Example                 |
| ------ | ----------------------- |
| Metric | request.duration        |
| Metric | request.count           |
| Trace  | user → API → DB         |
| Span   | payment-service latency |


## Splunk Universal Forwarder

### Step 1: Install Universal Forwarder
```
wget -O splunkforwarder.tgz https://download.splunk.com/products/universalforwarder/releases/latest/linux/splunkforwarder.tgz
tar -xvzf splunkforwarder.tgz -C /opt
```
### Step 2: Start & enable boot start
```
/opt/splunkforwarder/bin/splunk start --accept-license
/opt/splunkforxwarder/bin/splunk enable boot-start
```
### Step 3: Configure where to send logs
```
/opt/splunkforwarder/bin/splunk add forward-server splunk-indexer:9997
```
### Step 4: Add log files to monitor
## Example: system logs
```
/opt/splunkforwarder/bin/splunk add monitor /var/log/messages
/opt/splunkforwarder/bin/splunk add monitor /var/log/syslog
```
## Example: application logs
```
/opt/splunkforwarder/bin/splunk add monitor /var/log/myapp/app.log
```
### Step 5: Verify in Splunk
```
index=main host=my-server
```
### Why Universal Forwarder is best for logs
- Handles log rotation
- Backpressure handling
- Guaranteed delivery
- Secure (TLS)
- Low CPU/memory
- Battle-tested at scale
- This is why every enterprise uses it.


### What does the SignalFx OpenTelemetry Collector exactly collect?
- The SignalFx OpenTelemetry Collector does NOT “magically monitor everything.”
- It collects only what is explicitly sent or scraped through configured receivers.
```
Sources → Receivers → Processors → Exporters → SignalFx
```
- Sources = apps, OS, Kubernetes, cloud
- Receivers = how data enters the collector
- Processors = enrich / batch / filter
- Exporters = where data is sent

### 1️⃣ Metrics it collects (WHAT metrics?)

### A. Infrastructure / Host metrics (VMs & nodes)

### Collected only if host receivers are enabled:

- CPU: cpu.utilization, cpu.time, cpu.load (1m/5m/15m)
- Memory: memory.used, memory.available, memory.utilization
- Disk: disk.used, disk.free, disk.io, disk.latency
- Network: network.rx/tx bytes, packet drops, errors

### 📌 Source:
- Linux host
- EC2 / VM
- Kubernetes node (via kubelet)

### B. Kubernetes metrics (cluster state & health)

- Collected only if k8s receivers are enabled:
- Node: node.ready, node.memory_pressure, node.disk_pressure, node.pid_pressure
- Pod: pod.restarts, pod.cpu.throttling, pod.memory.usage, pod.phase (Running / Pending / Failed)
- Workloads: deployment.desired_replicas, deployment.available_replicas, rollout status

### 📌 Source:
- kubelet
- cAdvisor
- kube-state-metrics
- Kubernetes API

### C. Application metrics (Golden Signals ⭐)
- Collected only if the application is instrumented:
- Latency: http.server.duration (P50/P95/P99), db.query.duration, Traffic, request.count
- RPS
- Errors: http.status_code (4xx / 5xx), exception.count, timeout.count
- Saturation: thread pool usage, DB connection pool usage, queue depth

### 📌 Source:
- OpenTelemetry SDK inside the app

👉 If the app is NOT instrumented → no app metrics

### 2️⃣ Traces it collects (WHEN do traces appear?)

- The collector does not generate traces itself.
- It collects traces only when:
 - Your app is instrumented with OpenTelemetry
 - Traces are sent via OTLP

### What a trace contains
- trace_id
- spans (API, DB, cache, external calls)
- per-span latency
- error flags

### 📌 Source:
- App → OpenTelemetry SDK → OTLP → Collector


👉 No SDK = no traces
👉 No OTLP receiver = no traces

### 📌 VM / EC2 (Non-Kubernetes workloads)
### What to deploy & why

| Signal      | Tool to Use                                       | What it Collects                                  | How You Deploy                          |
| ----------- | ------------------------------------------------- | ------------------------------------------------- | --------------------------------------- |
| **Metrics** | **SignalFx OpenTelemetry Collector (agent mode)** | CPU, memory, disk, network, load avg, app metrics | Install as OS service (`systemd`)       |
| **Traces**  | **OpenTelemetry SDK + OTel Collector**            | Distributed traces, spans, latency                | Instrument app + send OTLP to collector |
| **Logs**    | **Splunk Universal Forwarder**                    | System logs, app logs                             | Install UF as background service        |


### 📌 Kubernetes (Containerized workloads)
### What to deploy & why

| Signal                    | Tool to Use                            | What it Collects                           | How You Deploy            |
| ------------------------- | -------------------------------------- | ------------------------------------------ | ------------------------- |
| **Metrics (Infra + K8s)** | **SignalFx OTel Collector**            | Node, pod, kube-state, CPU/mem, throttling | **DaemonSet** (via Helm)  |
| **Metrics (App)**         | **OpenTelemetry SDK**                  | RPS, latency, errors, saturation           | Instrument app containers |
| **Traces**                | **OpenTelemetry SDK + OTel Collector** | Distributed traces                         | App → OTLP → Collector    |
| **Logs**                  | **Fluent Bit**                         | Pod/container stdout logs                  | **DaemonSet**             |
| **Log Store**             | **Splunk**                             | Search, RCA, retention                     | HEC ingestion             |

### 📌 How each component is deployed (quick cheat)

| Component                  | VM / EC2             | Kubernetes           |
| -------------------------- | -------------------- | -------------------- |
| SignalFx OTel Collector    | OS package + systemd | Helm → DaemonSet     |
| OpenTelemetry SDK          | App runtime config   | App container config |
| Splunk Universal Forwarder | Installed on VM      | ❌ Not used           |
| Fluent Bit                 | ❌ Not used           | DaemonSet            |
| Logs destination           | Splunk Indexers      | Splunk HEC           |


### Fluentd vs Fluent Bit (real-world choice)
| Feature          | Fluentd               | Fluent Bit          |
| ---------------- | --------------------- | ------------------- |
| Resource usage   | Heavy (Ruby)          | Lightweight (C)     |
| Memory footprint | High                  | Very low            |
| Performance      | Good                  | Excellent           |
| Plugins          | Very rich             | Most common         |
| Best for         | Complex log pipelines | Kubernetes clusters |

- Fluent Bit runs as a DaemonSet, tails container stdout logs from /var/log/containers, enriches them with Kubernetes metadata, and sends them to Splunk via HEC.

### Fluent Bit Raw DaemonSet

### ConfigMap
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: fluent-bit-config
  namespace: logging
data:
  fluent-bit.conf: |
    [SERVICE]
        Flush 1
        Log_Level info

    @INCLUDE input.conf
    @INCLUDE filter.conf
    @INCLUDE output.conf

  input.conf: |
    [INPUT]
        Name tail
        Path /var/log/containers/*.log
        Parser cri
        Tag kube.*

  filter.conf: |
    [FILTER]
        Name kubernetes
        Match kube.*
        Merge_Log On

  output.conf: |
    [OUTPUT]
        Name splunk
        Match *
        Host splunk-hec.example.com
        Port 8088
        Splunk_Token ${SPLUNK_HEC_TOKEN}
        TLS On
```
### DaemonSet
```
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluent-bit
  namespace: logging
spec:
  selector:
    matchLabels:
      app: fluent-bit
  template:
    metadata:
      labels:
        app: fluent-bit
    spec:
      containers:
      - name: fluent-bit
        image: fluent/fluent-bit:2.2
        env:
        - name: SPLUNK_HEC_TOKEN
          valueFrom:
            secretKeyRef:
              name: splunk-hec-secret
              key: token
        volumeMounts:
        - name: varlog
          mountPath: /var/log
        - name: config
          mountPath: /fluent-bit/etc
      volumes:
      - name: varlog
        hostPath:
          path: /var/log
      - name: config
        configMap:
          name: fluent-bit-config
```

### 🔍 What logs Fluent Bit collects (exactly)

| Source           | Path                        |
| ---------------- | --------------------------- |
| Container stdout | `/var/log/containers/*.log` |
| Pod metadata     | Kubernetes API              |
| Node info        | kubelet                     |

- Each log is enriched with:
 - pod name
 - namespace
 - container
 - labels
 - node

### 🔗 Correlating logs with traces (IMPORTANT)
- Make sure app logs include:
```
{
  "service.name": "payment-service",
  "trace_id": "abc123",
  "span_id": "def456"
}
```

Then:
- SignalFx → trace
- Splunk → logs
- Same trace_id = instant RCA 🔥

### Observability Tools Comparison Table

| Area                        | **Splunk + SignalFx**                      | **Prometheus + Grafana** | **Elastic Stack (ELK)** | **Datadog**      | **New Relic**         |
| --------------------------- | ------------------------------------------ | ------------------------ | ----------------------- | ---------------- | --------------------- |
| **Primary strength**        | Enterprise logs + real-time observability  | Metrics & dashboards     | Logs & search           | All-in-one SaaS  | APM & monitoring      |
| **Logs**                    | ⭐ Best-in-class (scale, search, retention) | ❌ Not native             | Good but ops-heavy      | Good             | Good                  |
| **Metrics**                 | ⭐ Real-time, high-cardinality              | Good (DIY scaling)       | Limited                 | Good             | Good                  |
| **Traces / APM**            | Good (OTel-based)                          | Basic (needs addons)     | Limited                 | Good             | ⭐ Strong              |
| **SLO & burn-rate alerts**  | ⭐ Native, SRE-focused                      | Manual & complex         | ❌ No                    | Basic            | Basic                 |
| **Scales to TBs/day**       | ⭐ Yes (proven)                             | Hard                     | Hard                    | Yes (costly)     | Yes                   |
| **Operational overhead**    | Low (managed SaaS)                         | High (self-managed)      | High (tuning, JVM)      | Low              | Low                   |
| **Compliance / audit logs** | ⭐ Excellent                                | ❌ No                     | Medium                  | Medium           | Medium                |
| **Best for**                | Large enterprises, SRE teams               | Small/medium systems     | Log-centric teams       | SaaS-first teams | APM-heavy use cases   |
| **Cost**                    | 💸 High                                    | 🆓 Low                   | 🆓 Medium               | 💸 Medium–High   | 💸 Medium             |
| **Why teams choose it**     | Reliability + speed at scale               | Flexibility & cost       | Searchable logs         | Ease of use      | App performance focus |
