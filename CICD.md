# CI/CD PIPELINE FlOW
### 🚀 What is CI/CD?
- CI/CD = Continuous Integration + Continuous Delivery / Deployment
- Every time a developer writes code, CI/CD automatically builds, tests, secures, and deploys it.

### 1️⃣ Continuous Integration (CI)
- CI is the practice of automatically building and testing code whenever developers push changes.
- Goal: Catch problems early.
- Whenever a developer pushes code to Git:
	- What happens automatically?
		- ✅ Code is pulled from repo
		- 🧪 Tests are run
		- 🔍 Code quality & security checks
		- 📦 Build artifacts created
	- Why CI matters
		- No “it works on my machine” ❌
		- Bugs are caught in minutes, not weeks
		- Team can merge code confidently
    ```
    Developer → Git Push → Build → Test → Success/Fail
    ```
 
### 2️⃣ Continuous Delivery (CD)
- Code is automatically prepared for release but requires manual approval to deploy.
- Goal: Keep the app always ready to deploy.
	- App is built
	- Tested
	- Security-scanned
	- Stored as a release artifact
🚦 Deployment may require manual approval
```
Build → Test → Package → Ready for Production (Manual Approval)
```
### 3️⃣ Continuous Deployment (CD – advanced)
- Code is automatically deployed to production without manual approval.
- Goal: Zero human intervention.
- If all checks pass:
	➡️ Automatically deploy to production
- Used by companies like Netflix, Amazon, Google.
```
Build → Test → Deploy to Production (Fully Automatic)
```

| Continuous Delivery         | Continuous Deployment |
| --------------------------- | --------------------- |
| Manual approval before prod | Fully automatic       |
| Safer                       | Faster                |
| Used in enterprises         | Used in startups      |

```
1. Developer pushes code
2. CI runs:
   - Build
   - Unit test
   - Static scan
3. CD runs:
   - Build Docker image
   - Push to registry
   - Deploy to Kubernetes
```

### 1️⃣ COMPLETE PROJECT STRUCTURE
```bash
nodejs-ci-cd/
├── src/
│   ├── app.js
│   ├── server.js
│   ├── routes/
│   │   └── health.js
│   ├── config/
│   │   └── db.js
│   └── controllers/
│       └── health.controller.js
│
├── tests/
│   └── health.test.js
│
└── k8s/
    ├── dev/
    │   ├── deployment.yaml
    │   └── service.yaml
    ├── stage/
    │   ├── deployment.yaml
    │   └── service.yaml
    └── prod/
        ├── deployment.yaml
        └── service.yaml
│
├── Dockerfile
├── .gitlab-ci.yml
├── .gitignore
├── package.json
└── README.md
```

### Why this structure?
	- src/ → application code
	- tests/ → automated testing
	- Dockerfile → package app
	- .gitlab-ci.yml → pipeline steps
	- k8s/ → how app runs in Kubernetes
	- env folders → same app, different environments

### 🚶‍♂️ STAGE 0: DEVELOPER PUSHES CODE

- What happens
- Developer writes code and pushes it.
```
git push origin main
```
### WHY this stage exists
- Git is the single source of truth
- Every change is tracked
- Triggers automation
➡️ This push starts the CI/CD pipeline automatically

### ⚙️ STAGE 1: INSTALL DEPENDENCIES
```
npm install
```
- File involved
	- package.json

### Why we run this
- Ensure required libraries exist
- Catch broken or missing dependencies early

### What happens internally
- Reads dependencies
- Downloads packages
- Fails if versions conflict

❌ If this fails → STOP PIPELINE

### 🧪 STAGE 2: RUN TESTS
```
npm test
```
- File involved
	- tests/app.test.js

### Why we run this
- Verify app logic works
- Ensure APIs respond correctly
- Prevent broken code from deploying

### What is tested
- API response
- Status codes
- Logic correctness
❌ If tests fail → STOP PIPELINE

### 🧹 STAGE 3: CODE QUALITY CHECK (LINT)
```
npm run lint
```
- File involved
	- .eslintrc (or default rules)

### WHY we run this
- Catch bad coding practices
- Prevent runtime bugs
- Maintain readable, clean code

### What lint checks
- Syntax errors
- Unused variables
- Undefined variables
- Incorrect formatting
❌ If lint fails → STOP PIPELINE

### 🔐 STAGE 4: SECURITY & QUALITY SCAN

```
sonar-scanner
```

### WHY we run this
- Detect security issues early
- Find risky coding patterns
- Enforce quality standards

### What it checks
- Vulnerabilities
- Code smells
- Test coverage
❌ If quality gate fails → STOP PIPELINE

### 📦 STAGE 5: BUILD DOCKER IMAGE
```dockerfile
FROM node:18
WORKDIR /app
COPY package*.json .
RUN npm install
COPY . .
CMD ["node", "src/server.js"]
```
- Command
```docker
docker build -t nodejs-app:1.0 .
```

### WHY we do this
- Package app + runtime together
- Ensure same behavior everywhere
- Remove “works on my machine” issue
✔ Docker image = deployable unit

### 📤 STAGE 6: PUSH IMAGE TO REGISTRY
```docker
docker push registry/nodejs-app:1.0
```

### WHY we do this
- Store image centrally
- Reuse same image for all environments
- Enable rollback
✔ Image is immutable

### ☸️ STAGE 7: DEPLOY TO DEV ENVIRONMENT
- File involved
	- k8s/dev/deployment.yaml
 ```yaml
containers:
- name: app
  image: registry/nodejs-app:1.0
```
```bash
kubectl apply -f k8s/dev/
```

### WHY we do this
- Run app in Kubernetes
- Allow developers to test
- Catch environment issues early
✔ Pod starts in DEV

```css
Code push
 → install
 → test
 → lint
 → scan
 → docker build
 → push image
 → deploy dev
 → approve
 → deploy stage
 → approve
 → deploy prod
```

### ✅ .gitlab-ci.yml (END-TO-END, SIMPLE & CLEAR)

```bash
stages:
  - install
  - test
  - build
  - push
  - deploy_dev
  - deploy_stage
  - deploy_prod

variables:
  APP_NAME: nodejs-app
  IMAGE_TAG: $CI_COMMIT_SHORT_SHA
  DOCKER_IMAGE: $CI_REGISTRY_IMAGE/$APP_NAME:$IMAGE_TAG

# ----------------------------
# STAGE 1: INSTALL DEPENDENCIES
# ----------------------------
install_dependencies:
  stage: install
  image: node:18
  script:
    - npm install
  artifacts:
    paths:
      - node_modules/
  only:
    - main

# ----------------------------
# STAGE 2: RUN TESTS
# ----------------------------
run_tests:
  stage: test
  image: node:18
  script:
    - npm test
  only:
    - main

# ----------------------------
# STAGE 3: BUILD DOCKER IMAGE
# ----------------------------
docker_build:
  stage: build
  image: docker:24
  services:
    - docker:24-dind
  script:
    - docker build -t $DOCKER_IMAGE .
  only:
    - main

# ----------------------------
# STAGE 4: PUSH IMAGE TO REGISTRY
# ----------------------------
docker_push:
  stage: push
  image: docker:24
  services:
    - docker:24-dind
  script:
    - docker login -u "$CI_REGISTRY_USER" -p "$CI_REGISTRY_PASSWORD" $CI_REGISTRY
    - docker push $DOCKER_IMAGE
  only:
    - main

# ----------------------------
# STAGE 5: DEPLOY TO DEV
# ----------------------------
deploy_dev:
  stage: deploy_dev
  image: bitnami/kubectl:latest
  script:
    - kubectl config use-context $DEV_KUBE_CONTEXT
    - kubectl apply -f k8s/dev/
  only:
    - main

# ----------------------------
# STAGE 6: DEPLOY TO STAGE (MANUAL)
# ----------------------------
deploy_stage:
  stage: deploy_stage
  image: bitnami/kubectl:latest
  script:
    - kubectl config use-context $STAGE_KUBE_CONTEXT
    - kubectl apply -f k8s/stage/
  when: manual
  only:
    - main

# ----------------------------
# STAGE 7: DEPLOY TO PROD (MANUAL)
# ----------------------------
deploy_prod:
  stage: deploy_prod
  image: bitnami/kubectl:latest
  script:
    - kubectl config use-context $PROD_KUBE_CONTEXT
    - kubectl apply -f k8s/prod/
  when: manual
  only:
    - main
```

| Stage                        | Purpose (Why we do it)       | **Node.js**             | **Python**                            | **Java (Maven)**       | **Java (Gradle)**  | **Go**            | **.NET**                   |
| ---------------------------- | ---------------------------- | ----------------------- | ------------------------------------- | ---------------------- | ------------------ | ----------------- | -------------------------- |
| **Dependency File**          | Defines app dependencies     | `package.json`          | `requirements.txt` / `pyproject.toml` | `pom.xml`              | `build.gradle`     | `go.mod`          | `.csproj`                  |
| **Install Dependencies**     | Ensure app can run           | `npm install`           | `pip install -r requirements.txt`     | `mvn clean install`    | `gradle build`     | `go mod download` | `dotnet restore`           |
| **What this does**           | Downloads required libraries | npm packages            | Python libraries                      | JAR dependencies       | JAR dependencies   | Go modules        | NuGet packages             |
| **Unit / Integration Tests** | Verify logic & APIs work     | `npm test`              | `pytest`                              | `mvn test`             | `gradle test`      | `go test ./...`   | `dotnet test`              |
| **Test Framework**           | Test execution engine        | Jest / Mocha            | PyTest                                | JUnit                  | JUnit              | Built-in          | xUnit / NUnit              |
| **Lint / Code Quality**      | Catch bugs early             | `npm run lint` (ESLint) | `flake8` / `pylint`                   | `checkstyle`           | `spotbugs`         | `golangci-lint`   | `dotnet format`            |
| **What lint checks**         | Code smells & bad practices  | Syntax, unused vars     | Formatting, logic                     | Style, conventions     | Style, bugs        | Unsafe patterns   | Formatting, rules          |
| **Dependency Security Scan** | Find vulnerable libraries    | `npm audit`             | `safety`                              | `mvn dependency-check` | `dependency-check` | `govulncheck`     | `dotnet list package`      |
| **SAST / Code Scan**         | Detect security flaws        | SonarQube, Checkmarx    | SonarQube, Checkmarx                  | SonarQube, Checkmarx   | SonarQube          | SonarQube         | SonarQube                  |
| **Build / Compile Stage**    | Create deployable output     | Not compiled            | Not compiled                          | `mvn package`          | `gradle build`     | `go build`        | `dotnet build`             |
| **Build Output**             | What gets packaged           | JS files                | `.py` / wheel                         | `.jar` / `.war`        | `.jar`             | Single binary     | `.dll`                     |
| **Artifact Usage**           | Share output between stages  | `node_modules/`         | `.venv/`                              | `.jar`                 | `.jar`             | Binary            | `.dll`                     |
| **Dockerfile Base Image**    | Runtime environment          | `node:18`               | `python:3.x`                          | `openjdk:17`           | `openjdk:17`       | `golang:1.x`      | `mcr.microsoft.com/dotnet` |
| **Docker Build**             | Package app + runtime        | `docker build`          | `docker build`                        | `docker build`         | `docker build`     | `docker build`    | `docker build`             |
| **Docker Image Content**     | What container runs          | Node + app              | Python + app                          | JVM + JAR              | JVM + JAR          | Binary            | .NET runtime               |
| **Image Registry**           | Store images                 | GitLab / ECR / Nexus    | Same                                  | Same                   | Same               | Same              | Same                       |
| **Kubernetes Deploy Cmd**    | Deploy to cluster            | `kubectl apply`         | `kubectl apply`                       | `kubectl apply`        | `kubectl apply`    | `kubectl apply`   | `kubectl apply`            |
| **K8s Deployment Uses**      | What K8s runs                | Docker image            | Docker image                          | Docker image           | Docker image       | Docker image      | Docker image               |
| **Secrets Handling**         | DB & credentials             | K8s Secrets / IAM       | K8s Secrets / IAM                     | K8s Secrets / IAM      | K8s Secrets / IAM  | K8s Secrets / IAM | K8s Secrets / IAM          |
| **Final Runtime**            | Where app runs               | Pod (Container)         | Pod (Container)                       | Pod (Container)        | Pod (Container)    | Pod (Container)   | Pod (Container)            |

### 🟡 Python — Dockerfile

```dockerfile
FROM node:18-alpine

WORKDIR /app

# Install dependencies
COPY package*.json ./
RUN npm install --production

# Copy source code
COPY . .

EXPOSE 3000
CMD ["node", "src/server.js"]
```

### 🔵 Java (Maven) — Dockerfile
```dockerfile
# Build stage
FROM maven:3.9.6-eclipse-temurin-17 AS build
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN mvn clean package -DskipTests

# Runtime stage
FROM eclipse-temurin:17-jre
WORKDIR /app
COPY --from=build /app/target/app.jar app.jar

CMD ["java", "-jar", "app.jar"]
```
### 🔵 Java (Gradle) — Dockerfile
```dockerfile
# Build stage
FROM gradle:8.5-jdk17 AS build
WORKDIR /app
COPY . .
RUN gradle build -x test

# Runtime stage
FROM eclipse-temurin:17-jre
WORKDIR /app
COPY --from=build /app/build/libs/app.jar app.jar

CMD ["java", "-jar", "app.jar"]
```
### 🟣 Go — Dockerfile
```dockerfile
# Build stage
FROM golang:1.22-alpine AS build
WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o app

# Runtime stage
FROM alpine:latest
WORKDIR /app
COPY --from=build /app/app .

CMD ["./app"]
```
### 🔴 .NET — Dockerfile
```dockerfile
# Build stage
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /app
COPY . .
RUN dotnet publish -c Release -o out

# Runtime stage
FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY --from=build /app/out .

CMD ["dotnet", "app.dll"]
```

### ✅ OPTION 1: KUBECONFIG FILE (MOST COMMON)
- To talk to a cluster, you must provide:
	- Cluster address (API server)
	- Authentication (token / cert)
	- Authorization (RBAC permissions)
 - All of this lives in a file called: ```kubeconfig```

- A kubeconfig file tells kubectl:
	- Which cluster to talk to
	- Who you are
	- What you’re allowed to do
 ```yaml
apiVersion: v1
clusters:
- cluster:
    server: https://eks.amazonaws.com
    certificate-authority-data: LS0tLS1CRUdJTi...
  name: eks-prod

users:
- name: ci-user
  user:
    token: eyJhbGciOi...

contexts:
- context:
    cluster: eks-prod
    user: ci-user
  name: prod-context
```

- DevOps creates a kubeconfig file
- Uploads it to CI/CD as a secret variable
- CI writes it to disk at runtime

```yaml
variables:
  KUBECONFIG: /builds/kubeconfig

before_script:
  - echo "$KUBE_CONFIG_DATA" | base64 -d > $KUBECONFIG
```

### ✅ OPTION 2: KUBERNETES SERVICE ACCOUNT TOKEN (K8s-NATIVE)
- CI/CD uses a Kubernetes ServiceAccount token to authenticate to the Kubernetes API.

### 🟦 STEP 1: CREATE A SERVICE ACCOUNT
```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: ci-deployer
  namespace: default
```
- WHY
  - ServiceAccount = identity inside Kubernetes
  - Just like a user, but for machines

### 🟦 STEP 2: GRANT PERMISSIONS (RBAC)
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: deploy-role
rules:
- apiGroups: ["apps"]
  resources: ["deployments"]
  verbs: ["get", "create", "update"]
```
- Role binding
```yaml
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: deploy-binding
subjects:
- kind: ServiceAccount
  name: ci-deployer
roleRef:
  kind: Role
  name: deploy-role
  apiGroup: rbac.authorization.k8s.io
```

### WHY
- Controls what CI can do
- CI cannot:
	- Delete cluster
	- Read secrets (unless allowed)
👉 Principle of least privilege

### 🟦 STEP 3: EXTRACT THE TOKEN
- Kubernetes auto-creates a token for the ServiceAccount.
```yaml
kubectl get secret
kubectl describe secret ci-deployer-token-xxxx
```
You get:
- Token
- CA certificate
- API server URL

### 🟦 STEP 4: STORE TOKEN IN CI/CD (SECURELY)
- In GitLab CI → Variables:

| Variable         | Value                                                |
| ---------------- | ---------------------------------------------------- |
| `K8S_TOKEN`      | ServiceAccount token                                 |
| `K8S_API_SERVER` | [https://cluster-endpoint](https://cluster-endpoint) |
| `K8S_CA_CERT`    | Base64 cert                                          |

### 🟦 STEP 5: CI USES TOKEN TO DEPLOY

```yaml
kubectl --token=$K8S_TOKEN \
  --server=$K8S_API_SERVER \
  --certificate-authority=$K8S_CA_CERT \
  apply -f k8s/dev/
```
### WHAT HAPPENS
- CI authenticates as ci-deployer
- Kubernetes checks RBAC
- If allowed → deploy succeeds

### ✅ OPTION 3: CLOUD IAM (EKS / GKE / AKS)
- CI/CD uses cloud identity (IAM) instead of Kubernetes tokens.

| Problem           | Solved by IAM |
| ----------------- | ------------- |
| Token rotation    | Automatic     |
| Secrets leakage   | No secrets    |
| Cloud integration | Native        |
| Compliance        | Strong        |



```
Code Push
  ↓
Build
  ↓
SonarQube (Quality Gate)
  ↓
Checkmarx (SAST – Code Security)
  ↓
Docker Build
  ↓
Trivy (Image / IaC Scan)
  ↓
Deploy
```

```
SONAR_HOST_URL
SONAR_TOKEN

CHECKMARX_SERVER
CHECKMARX_USERNAME
CHECKMARX_PASSWORD
CHECKMARX_TEAM
```

```
stages:
  - build
  - quality
  - security
  - image
  - deploy

variables:
  GIT_DEPTH: "0"
  DOCKER_IMAGE: "$CI_REGISTRY_IMAGE:$CI_COMMIT_SHORT_SHA"

# --------------------
# BUILD STAGE
# --------------------
build:
  stage: build
  image: maven:3.9-eclipse-temurin-17
  script:
    - mvn clean package -DskipTests
  artifacts:
    paths:
      - target/*.jar
  only:
    - merge_requests
    - main

# --------------------
# SONARQUBE – CODE QUALITY
# --------------------
sonarqube_scan:
  stage: quality
  image: sonarsource/sonar-scanner-cli:latest
  script:
    - sonar-scanner
      -Dsonar.projectKey=$CI_PROJECT_NAME
      -Dsonar.sources=.
      -Dsonar.host.url=$SONAR_HOST_URL
      -Dsonar.login=$SONAR_TOKEN
  allow_failure: false
  only:
    - merge_requests
    - main

# --------------------
# CHECKMARX – SAST SECURITY
# --------------------
checkmarx_scan:
  stage: security
  image: checkmarx/cx-flow:latest
  script:
    - java -jar cx-flow.jar
      --scan
      --project-name "$CI_PROJECT_NAME"
      --cx-server "$CHECKMARX_SERVER"
      --cx-username "$CHECKMARX_USERNAME"
      --cx-password "$CHECKMARX_PASSWORD"
      --cx-team "$CHECKMARX_TEAM"
      --branch "$CI_COMMIT_REF_NAME"
  allow_failure: false
  only:
    - merge_requests
    - main

# --------------------
# DOCKER IMAGE BUILD
# --------------------
docker_build:
  stage: image
  image: docker:24
  services:
    - docker:24-dind
  script:
    - docker build -t $DOCKER_IMAGE .
  only:
    - main

# --------------------
# TRIVY – IMAGE & IaC SCAN
# --------------------
trivy_scan:
  stage: image
  image: aquasec/trivy:latest
  script:
    - trivy image --severity HIGH,CRITICAL --exit-code 1 $DOCKER_IMAGE
  allow_failure: false
  only:
    - main

# --------------------
# DEPLOY
# --------------------
deploy:
  stage: deploy
  script:
    - echo "Deploying to production..."
  only:
    - main
```
