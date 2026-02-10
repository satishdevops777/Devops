# SRE
## What is SRE (in simple words)
- Site Reliability Engineering ensures that whatever is live for users works reliably, securely, and within agreed SLAs, even as new features and changes are continuously deployed.
- SRE focuses on:
  - Reliability
  - Availability
  - Performance
  - Security
  - Automation
  - Fast recovery from failures

## 1. User Experience Monitoring (Very Clear)
### Scenario (Production Reality)
- You work for an e-commerce company.
- Users complain:
  - “Payment page is very slow, sometimes it fails.”
  - But:
    - App is running
    - Pods are healthy
    - No crash

### Step 1: What SRE Sees (Monitoring)
- You open dashboards and check:
  - API Response Time
    - /payment API normally = 300 ms
    - Now = 4–5 seconds
  - HTTP Status Codes
    - Mostly 200 OK
    - Some 504 (timeout)

⚠️ Important:
- App is not down, but users are impacted.

### Step 2: Pattern Detection
- You check:
  - Time → Happens only during peak traffic
  - Geography → Only users from Europe
  - Certificate → Valid
  - Recent deployment → None
- Now you know:
  - This is a performance issue, not an outage.

### Step 3: Root Cause (Deep)
- Using distributed tracing:
  - Payment API → DB call takes 3 seconds
  - DB CPU is high
  - One SQL query is slow

### Step 4: Fix
- Optimize query
- Add DB read replica
- Cache response

### Step 5: Result
- API latency back to 300 ms
- No more complaints

### WHY THIS IS SRE
👉 Because reliability = user experience, not just uptime.


## 2: Application Auto-Recovery (Self-Healing)
### Scenario
- A Java microservice starts consuming too much memory.

### Step 1: What Monitoring Detects
- Memory usage → 95%
- Garbage collection → very frequent
- API latency → increasing

### Step 2: What Happens Automatically
- Because SRE configured it earlier:
- Kubernetes kills unhealthy pod
- New pod starts
- Load balancer shifts traffic

### Step 3: User Impact
- Maybe 1–2 slow requests
- No outage

### Step 4: SRE Follow-Up
- After incident:
- Increase memory limit
- Tune JVM
- Add alert at 80%

### WHY THIS IS SRE
👉 Systems recover without humans.

## 3: Certificate Expiry (Very Common)
### Scenario
- TLS certificate expires at 12:00 AM.
- Without SRE Monitoring ❌
  - All HTTPS calls fail
  - Users see “Connection not secure”
  - Full outage
- With SRE Monitoring ✅
  - Step 1: Alert
    - Alert fired 7 days before expiry
  - Step 2: Action
    - Renew certificate
    - Deploy new cert
    - Test endpoints
- Result
  - No outage
  - Users never know

### WHY THIS IS SRE
👉 Prevent incidents before they happen.

## 4: Change Management (Deployment Failure)
### Scenario
- New release deployed to production.
- Step 1: Monitoring Detects
  - API error rate ↑
  - Checkout failures ↑
- Step 2: SRE Action
  - Check deployment time
  - Compare metrics before vs after
  - Rollback immediately
- Step 3: Communication
  - Inform product & management
  - Incident declared SEV-2
- Step 4: Postmortem
  - Bug in new logic
  - Add canary deployment

### WHY THIS IS SRE
👉 Fast rollback saves SLA.


## 5: Incident Management (SEV-1)
### Scenario
- Production is completely down.
- Step-by-Step Timeline
- 00:00
  - API health check fails
  - Alert triggered
- 00:02
  - On-call SRE acknowledges
- 00:05
  - Slack/email sent to stakeholders
- 00:08
  - DB not accepting connections
- 00:12
  - DB restarted
  - App recovers
- Total Downtime
  - 12 minutes
-  After Incident (RCA)
  - Root cause:
    - DB connection pool exhausted
  - Fix:
    - Increase pool
    - Add alert

### WHY THIS IS SRE
👉 Restore first, analyze later.

## 6: SLA / SLO / Error Budget (Crystal Clear)
### SLA
- 99.9% uptime per month
- Allowed downtime
  - ~43 minutes
- Incident
  - Downtime = 12 minutes
  - Remaining error budget = 43 – 12 = 31 minutes
- Decision
  - Deployments still allowed
  - But carefully

### WHY THIS IS SRE
👉 Reliability is measured, not guessed.


## 7: SOP (Runbook in Action)
### Alert
- API latency > 2 seconds
- SOP Steps
  - Check dashboard
  - Check recent deployments
  - Check DB latency
  - Scale service
  - Escalate if needed
- Result
  - Faster resolution
  - Less panic
 
## 8: Security Monitoring (Attack)
### Scenario
- Sudden spike in login failures.
- Step 1: Detection
  - 50k failed logins
  - Same IP range
  - Same usernames
- Step 2: Action
  - Enable rate limiting
  - Block IPs
  - Inform security team
- Result
  - Attack stopped
  - No breach
 
## 9: System Health (Disk Full)
### Scenario
- Disk usage = 98%.
- Impact
  - Logs cannot be written
  - App crashes
- Fix
  - Cleanup
  - Increase disk
  - Alert at 75%
 
## 10: Application Functionality (Silent Failure)
### Scenario (Dangerous)
- API returns 200 OK
- Orders not saved
- Detection
  - Synthetic order test fails
  - Business metric drops
- Fix
  - Rollback code
  - Add business-level monitoring


# SRE FLOW → KUBERNETES (END-TO-END, REAL PRODUCTION)
- Production Setup (Before Any Incident)
- You have:
  - Kubernetes cluster (prod)
  - Application = E-commerce Checkout
- Deployed as:
  - Deployment (pods)
  - Service
  - Ingress
- Monitoring + alerts already configured
- Users access:
```nginx
- Browser → Ingress → Service → Pod → Database
```
- SRE’s job starts after this goes live.


## STEP 1: USER REQUEST ENTERS KUBERNETES
### What User Does
- A customer clicks “Pay Now”.
- What Happens in Kubernetes
  - Request hits Ingress
  - Ingress forwards to Service
  - Service load-balances to a Pod
  - Pod calls Database
- At this moment:
  - Kubernetes thinks everything is healthy
  - Pods are running
  - Nodes are fine

## STEP 2: PROBLEM STARTS (USER EXPERIENCE ISSUE)
### What User Feels
- Page spins
- Payment takes long
- Sometimes fails
- Important
❗ App is not down
❗ Kubernetes is not crashing
❗ This is a performance reliability issue
- This is classic SRE territory.

## STEP 3: MONITORING DETECTS THE PROBLEM
### What Monitoring Sees
- Ingress Metrics
- /checkout latency:
  - Normal: 300 ms
  - Now: 4 seconds
- HTTP Status Codes
  - 90% → 200 OK
  - 10% → 504 Gateway Timeout
- Geography
  - Only users from Europe
- Kubernetes State
  - Pods: Running
  - Nodes: Healthy
  - No restarts

👉 This means:
Application is alive but unreliable

## STEP 4: ALERT FIRES (SRE ON-CALL)
### Alert message:
- “Checkout API latency > 3s for 5 minutes (SEV-2)”
- Why SEV-2?
  - Users impacted
  - Revenue impact
  - App not fully down
- SRE acknowledges alert.


## STEP 5: SRE DEBUGGING IN KUBERNETES (VERY PRACTICAL)
### 5.1 Check Pods
```ps
kubectl get pods
```
- All pods running.

### 5.2 Check Pod Resources
```ps
kubectl top pods
```
- CPU: OK
- Memory: OK
- So not a pod crash issue.

### 5.3 Check Pod Logs
```ps
kubectl logs checkout-pod
```
- Logs show:
  - “DB query took 3.2 seconds”

Now SRE knows:
👉 Problem is downstream (database)

## STEP 6: ROOT CAUSE FOUND
- What Actually Happened
  - Traffic increased
  - Database is overloaded
  - Pods waiting for DB response
  - Requests timing out at Ingress
- Kubernetes Is Doing Its Job
  - Pods running
  - Services routing
  - But capacity is insufficient
 
STEP 7: IMMEDIATE MITIGATION (RESTORE SERVICE)
What SRE Does FIRST

Goal = restore user experience fast

Option 1: Scale Pods
replicas: 3 → 8

Option 2: Enable HPA

Auto-scale pods based on CPU/latency

Traffic spreads across more pods.

Result (Within Minutes)

API latency drops to 400 ms

Timeouts stop

Users can pay again

✅ Incident mitigated

STEP 8: COMMUNICATION (VERY IMPORTANT)

SRE sends update:

Issue identified

Temporary mitigation applied

Monitoring continues

Stakeholders are calm because:
👉 Users are no longer blocked

STEP 9: POST-INCIDENT (RCA – AFTER FIX)

Now SRE does not stop here.

Root Cause

DB could not handle peak traffic

No alert on DB latency earlier

Permanent Fix

Add DB read replica

Optimize slow query

Add alert:

DB query time > 1s

Update capacity plan

STEP 10: ERROR BUDGET CHECK (SRE FUNDAMENTAL)
SLO

99.9% availability

Allowed downtime = 43 mins/month

Incident Impact

10 minutes of bad experience

Error budget remaining:

43 – 10 = 33 minutes


Decision:

Deployments allowed

But with caution

STEP 11: AUTOMATION ADDED (SRE MATURITY)

To avoid repeat incident:

HPA enabled permanently

Synthetic checkout tests added

DB latency alerts added

Dashboard updated

Now next time:
👉 Kubernetes auto-scales before users complain

STEP 12: WHY THIS IS “SRE FLOW”

This single incident covered:

✔ User experience monitoring
✔ Kubernetes observability
✔ Alerting
✔ Incident management
✔ Fast recovery
✔ Communication
✔ RCA
✔ Automation
✔ Capacity planning
