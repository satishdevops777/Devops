
## Terraform
This guide provides a detailed and comprehensive understanding of Terraform concepts, features, and integration with GitLab. It is designed to cover essential to advanced topics, including examples and explanations for each concept.

---

## 1. Variable Precedence
Terraform evaluates variables in the following order:
1. **Environment variables**
2. **.tfvars** file (if present)
3. **.tfvars.json**
4. **\*.auto.tfvars** or **\*.auto.tfvars.json**
5. **Any -var or -var-file options** on the CLI

- The order is CLI -var, then -var-file, environment variables, terraform.tfvars, *.auto.tfvars, and finally default values in variable definitions.”
  
  ```
  terraform apply -var="instance_type=t3.large"
  terraform apply -var-file="prod.tfvars"
  export TF_VAR_instance_type="t3.medium"
  ```
---

## 2. Data Types
Supported data types in Terraform:
- **string**: A sequence of Unicode characters (e.g., `"hello"`).
  ```
  variable "env" {
  type = string
  }
  ```
  
- **number**: A numeric value.
  ```
  variable "instance_count" {
  type = number
  }
  ```
- **bool**: True or false.
  ```
  variable "enable_monitoring" {
  type = bool
  }
  ```
- **list**: Ordered collection of same-type values. (e.g., `["a", 15, true]`).
  ```
  variable "subnet_ids" {
  type = list(string)
  }

  ["subnet-1", "subnet-2", "subnet-3"]
  ```
- **set**: A collection of unique values with no secondary identifiers.
  ```
  variable "availability_zones" {
  type = set(string)
  }

  ["ap-south-1a", "ap-south-1b"]
  ```
- **map**: A group of values identified by named labels (e.g., `{name="mabel", age=52}`).
- **null**: Represents absence or omission.

  ```
  variable "eks_config" {
    type = object({
      cluster_name = string
      version      = string
      node_groups  = map(object({
        instance_type = string
        min_size      = number
        max_size      = number
      }))
    })
  }
  ```
---

## 3. Count and `count.index`
- Use `count` and `count.index` to create identical resources with different names.
- Syntax: `[count.index]`.

  ```
  resource "aws_instance" "web" {
    count         = 3
    ami           = "ami-0abc123"
    instance_type = "t3.micro"
  }
  ```
  👉 Terraform creates:
   ```
    aws_instance.web[0]
    aws_instance.web[1]
    aws_instance.web[2]
    ```
- A zero-based index
- Automatically assigned for each instance
  ```
  resource "aws_instance" "web" {
    count         = 3
    ami           = "ami-0abc123"
    instance_type = "t3.micro"
  
    tags = {
      Name = "web-${count.index}"
    }
  }
  ```
  ```
  web-0
  web-1
  web-2
  ```
  ---

## 4. Conditional Expressions
Conditional expressions allow you to choose between two values based on a condition.
- Syntax: `condition ? true_value : false_value`.
- Example:
  ```hcl
  instance_type = var.environment == "development" ? "t2.micro" : "m5.large"
  ```

---

## 5. Terraform Functions
Built-in functions include:
- **Numeric functions**: `abs`, `ceil`, `floor`, `max`, `min`
- **String functions**: `concat`, `replace`, `split`, `tolower`, `toupper`
- **Collection functions**: `element`, `keys`, `length`, `merge`, `sort`
- **Filesystem functions**: `file`, `filebase64`

Example:
```hcl
lookup(map, key, default)
```

---

## 6. Local Values
Centralize values to avoid repetition and improve readability.
```hcl
locals {
  common_tags = {
    team = "payments"
  }
}
```
Reference: `tags = local.common_tags`.

---

## 7. Debugging in Terraform
- Enable logs for detailed outputs:
  ```bash
  export TF_LOG=TRACE
  export TF_LOG_PATH=PATHNAME
  ```
- Log levels: TRACE, DEBUG, INFO, WARN, ERROR.

- Terraform debugging involves using plan and validate to catch configuration issues, outputs and terraform console to inspect variable and local values, state commands to verify resource tracking, and TF_LOG for provider-level debugging. We also use refresh-only plans to detect state drift.”
---

## 8. Terraform Format
- Use `terraform fmt` to ensure consistent formatting of configuration files.

---

## 9. Data Sources
Fetch information defined outside Terraform.
Example:
```
data "<PROVIDER>_<TYPE>" "<NAME>" {
  # filters
}
data.aws_vpc.main.id

data "aws_ami" "amazon_linux" {
  most_recent = true
  owners      = ["amazon"]

  filter {
    name   = "name"
    values = ["amzn2-ami-hvm-*"]
  }
}

```

```hcl
data "local_file" "food" {
  filename = "${path.module}/filename"
}
```
- Data sources in Terraform allow us to fetch and reference existing infrastructure or external data without managing its lifecycle. They’re commonly used to look up resources like VPCs, subnets, AMIs, and account information and inject those values into managed resources.”
---

## 10. Terraform Graph
Generate a visual representation of dependencies.
```bash
terraform graph -type=plan | dot -Tpng > graph.png
```

---

## 11. Security Primer
- **Sensitive Parameters:** Prevent data exposure in logs or CLI.
  - Sensitive parameters prevent secret values (passwords, tokens, keys) from being: Printed in CLI output, Shown in logs, Exposed in terraform plan or apply
    ```
    variable "db_password" {
    type      = string
    sensitive = true
    }
    ```

⚠️ Important Reality Check

- Sensitive values are still stored in state
- They are just masked from display
📌 That’s why remote encrypted state is mandatory.

- **HashiCorp Vault:** Securely store secrets.

🤔 Why Vault?

- Terraform should not store secrets long-term.
- Vault provides:
  - Centralized secret storage
  - Encryption
  - Access control
  - Rotation
 
  ```
  provider "vault" {
  address = "https://vault.company.com"
  }

  data "vault_kv_secret_v2" "db" {
  mount = "secret"
  name  = "prod/db"
  }
  password = data.vault_kv_secret_v2.db.data["password"]
  ```
🔥 What This Solves
  - ✔ No secrets in Git
  - ✔ No secrets in tfvars
  - ✔ Central rotation
  - ✔ Audit logs

Terraform just reads secrets at runtime.

- **Terraform.lock.hcl:** Manages provider plugins and Terraform versions.
  ```
  provider "aws" {
    version = "~> 5.0"
  }

  terraform.lock.hcl
  ```
- With:
  - Exact version
  - Cryptographic hash
---

## 12. Terraform Cloud
- Manage runs in a consistent environment.
- Features: Access control, private registry, policy controls.
- **Sentinel:** Policy as code framework.
- **Remote Backend:** Includes Terraform Cloud, Local, and Remote backends.

---

## 13. Load Order & Semantics

1️⃣ Load order → In what sequence Terraform reads and evaluates things

2️⃣ Semantics → What Terraform means by variables, references, dependencies, and execution
- Extensions must be `.tf` or `.tf.json`.
- 
- Terraform follows this logical evaluation flow:
  - Load configuration files
  - Load providers & backend
  - Load input variables
  - Evaluate locals
  - Read data sources
  - Build dependency graph
  - Plan
  - Apply (graph-based execution)

- “Terraform loads all configuration files together, initializes providers and backends, evaluates variables and locals, reads data sources, and then builds a dependency graph. Planning and applying are done by walking this graph, not by file order or line order, allowing safe parallel execution while respecting dependencies.”

## 14. Terraform Validate
- Run `terraform validate` to ensure syntactic correctness.
- “terraform validate checks Terraform configurations for syntax and internal consistency without contacting providers or modifying infrastructure. It’s typically used early in development and in CI pipelines to catch errors before planning or applying changes.”
  
---

## 15. Resource Behavior and Meta Arguments
Customize resource behavior using meta-arguments:
- **depends_on**: Handle hidden dependencies.
- **count**: Create multiple resource instances.
- **for_each**: Create resources for each map or set item.
- **lifecycle**:
  - `create_before_destroy`: Create a new resource before destroying the old one.
      ```
      lifecycle {
        create_before_destroy = true
      }
      ```
    ✅ Zero-downtime updates
      Used for:
      - Load balancers
      - Launch templates
      - ASGs
      
  - `ignore_changes`: Ignore specific changes.
  - `replace_triggered_by`: Replace resource when dependencies change.


- `prevent_destroy`: Prevent accidental destruction.
    ```
      lifecycle {
      prevent_destroy = true
    }
    ```
- ✔ Protects:
  - Databases
  - Critical buckets
  - Stateful services
- Terraform will fail the plan if destruction is attempted.
---
- “Terraform meta-arguments like depends_on, count, for_each, and lifecycle allow us to control resource creation order, scale resources safely, and manage replacement behavior. We use for_each for stable production resources, depends_on only for hidden dependencies, and lifecycle rules like prevent_destroy and create_before_destroy to protect and update critical infrastructure safely.”

## 16. Terraform Taint
- Terraform taint marks a specific resource as damaged or unreliable, forcing Terraform to destroy and recreate it on the next apply.

- You use taint when:
  - A resource is corrupted
  - Manual changes broke it
  - Provisioning partially failed
  - Drift exists but Terraform can’t detect it
  - You want a clean rebuild without touching code

```bash
terraform taint aws_instance.web[0]
terraform untaint aws_instance.web # If you tainted by mistake: Removes taint flag
terraform apply -replace="resourcename.localname"
```
- “Terraform taint marks a resource as needing replacement, forcing Terraform to destroy and recreate it during the next apply. It’s useful when a resource is corrupted or partially provisioned but still appears healthy in state.”
- “Both terraform taint and -replace force resource recreation, but taint modifies the state and persists across runs, while terraform apply -replace is explicit, one-time, and safer. Modern Terraform workflows strongly prefer -replace.”
- Taint → “This resource is bad forever until fixed”
- Replace → “Replace this resource now, intentionally”
---

## 17. Terraform Modules
Enable reusable configurations with a standard structure:
- **main.tf**: Resource logic.
- **variables.tf**: Input variables.
- **outputs.tf**: Output values.


### 1️⃣ Root Module
- The current working directory
- Where you run terraform apply
- Calls other modules

### 2️⃣ Child Modules
- Reusable modules called by root or other modules
- Contain resources, variables, outputs
  ```
  modules/
  └── vpc/
      ├── main.tf
      ├── variables.tf
      ├── outputs.tf
      └── versions.tf
  ```


### 🔧 Example: VPC Module
```
modules/vpc/main.tf

resource "aws_vpc" "this" {
  cidr_block = var.cidr_block
  tags       = var.tags
}

modules/vpc/variables.tf

variable "cidr_block" {
  type = string
}

variable "tags" {
  type = map(string)
}

modules/vpc/outputs.tf

output "vpc_id" {
  value = aws_vpc.this.id
}

```
### 📞 Calling a Module (Root Module)
```
module "vpc" {
  source     = "./modules/vpc"
  cidr_block = "10.0.0.0/16"
  tags = {
    Env = "prod"
  }
}

usage: vpc_id = module.vpc.vpc_id

```
### 🌍 Module Sources
```
source = "./modules/vpc"
source = "git::https://github.com/org/vpc.git"
source = "terraform-aws-modules/vpc/aws"
source = "s3::https://bucket/vpc.zip"
```
- ✔ Local
- ✔ Git
- ✔ Terraform Registry
- ✔ S3

- “Terraform modules are reusable, self-contained configurations that encapsulate infrastructure logic. We use them to standardize infrastructure, reduce duplication, and enforce best practices across environments.”
---

## 18. Terraform Workspaces
Manage multiple environments with the same configuration while maintaining separate state files.
```bash
terraform workspace list
terraform workspace show
terraform workspace new dev
terraform workspace select dev
```
- Each workspace has:
  - Same .tf files
  - Same backend
  - Different state file
  ```
  s3://tf-state-bucket/app/default/terraform.tfstate
  s3://tf-state-bucket/app/dev/terraform.tfstate
  s3://tf-state-bucket/app/prod/terraform.tfstate
  ```
---

## 19. Remote State Management
- Terraform state keeps track of:
  - What resources exist
  - Their IDs
  - Dependencies
  - Metadata
- By default, state is stored locally: terraform.tfstate
- 👉 Remote state stores this file in a shared, centralized backend so teams can work safely together.
  - Without remote state:
    - ❌ Team conflicts
    - ❌ State overwrites
    - ❌ No locking
    - ❌ Secrets exposed
    - ❌ No collaboration
  
  - With remote state:
    - ✅ Team-safe
    - ✅ State locking
    - ✅ Secure storage
    - ✅ Auditable changes
    - ✅ CI/CD friendly

- Flow:
  - 1️⃣ Terraform reads backend config
  - 2️⃣ State stored remotely (S3 / Terraform Cloud / etc.)
  - 3️⃣ Lock acquired (DynamoDB / TFC)
  - 4️⃣ Plan / Apply runs
  - 5️⃣ Lock released

```
terraform {
  backend "s3" {
    bucket         = "company-terraform-state"
    key            = "prod/network/terraform.tfstate"
    region         = "ap-south-1"
    dynamodb_table = "terraform-locks"
    encrypt        = true
  }
}
```
- This gives you:
  - ✔ Central state
  - ✔ Encryption at rest
  - ✔ State locking

  ### 🤔 Why Locking?
    - Prevents:
      - Two people running apply at the same time
      - Corrupted state
      - Partial updates

### Why State Is Sensitive

- State may contain: Passwords, Tokens, Private IPs, ARNs, Resource metadata
- Security Best Practices
  - ✅ Enable encryption (KMS)
  - ✅ Restrict bucket access (IAM)
  - ✅ Enable versioning
  - ✅ Enable audit logs (CloudTrail)
  - ✅ Separate state per environment

- Backends determine where Terraform stores state.
- Use S3 with DynamoDB for state locking:
  ```hcl
  backend "s3" {
    bucket         = "my-terraform-state"
    key            = "terraform.tfstate"
    region         = "us-west-2"
    dynamodb_table = "state-lock"
  }
  ```


### 🆚 Local State vs Remote State

| Feature            | Local | Remote |
| ------------------ | ----- | ------ |
| Team collaboration | ❌     | ✅      |
| Locking            | ❌     | ✅      |
| Security           | ❌     | ✅      |
| CI/CD support      | ❌     | ✅      |
| Production ready   | ❌     | ✅      |

---

## 20. Comments in Terraform
- Single-line: `#` or `//`
- Multi-line: `/* ... */`

---

## 21. Zipmap
Create a map from keys and values.
```hcl
zipmap(["a", "b"], [1, 2])
```

---

## 22. Dynamic Blocks
Create repeatable nested blocks dynamically.
Example:
```hcl
dynamic "ingress" {
  for_each = var.allowed_ports
  content {
    from_port   = ingress.value
    to_port     = ingress.value
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
```
- “Dynamic blocks in Terraform allow us to generate repeated nested configuration blocks based on a collection. They’re commonly used for things like security group rules, listeners, or IAM rules, and help reduce duplication while keeping configurations flexible.”
---

## 23. Nested Modules
Encapsulate reusable functionality.
Example structure:
- `vpc/main.tf`
- `subnet/main.tf`

---

## 24. Debugging and Troubleshooting
Enable debug logs for troubleshooting:
```bash
TF_LOG=DEBUG terraform apply
```

---

## 25. Terraform and GitOps
Integrate Terraform with GitOps tools like ArgoCD for infrastructure as code delivery.

---

## 26. Terraform State Versioning
Track changes and ensure consistency by versioning state files.

---

## 27. Custom Providers
Develop custom providers for unsupported APIs using the Terraform SDK.

---

## 28. Multi-Cloud Deployments
- Deploy resources across multiple cloud providers in the same configuration.
- “Terraform enables multi-cloud deployments by allowing multiple providers in a single configuration and using a consistent IaC workflow. We typically separate state per cloud, abstract infrastructure into provider-specific modules, and use shared patterns for networking, security, and automation.”
---

## 29. Resource Targeting
Apply changes only to specific resources without affecting others:
```bash
terraform apply -target=aws_instance.example
```

---

## 30. Complex Outputs
- Export structured data like lists or maps.
Example:
```hcl
output "instance_details" {
  value = {
    ids   = aws_instance.example.*.id
    ips   = aws_instance.example.*.public_ip
    names = aws_instance.example.*.tags.Name
  }
}
```

- “Terraform outputs expose values from a configuration or module so they can be consumed by users, automation, or other modules. They’re essential for module composition and cross-stack communication, and sensitive outputs can be masked from CLI output.”

---

## 31. Managing Secrets with Vault
Store sensitive data securely.
Example:
```hcl
provider "vault" {
  address = "https://vault.example.com"
}

data "vault_generic_secret" "example" {
  path = "secret/data/myapp"
}
```

---

## 32. Terraform and Docker
Manage Docker containers with Terraform.
Example:
```hcl
provider "docker" {}

resource "docker_image" "nginx" {
  name = "nginx:latest"
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.name
  name  = "nginx-container"
  ports {
    internal = 80
    external = 8080
  }
}
```

---


# Terraform follows this lifecycle:
```
init → validate → plan → apply → manage → destroy
```
### 📌 Purpose
- Initializes the working directory.

```
terraform init
```

### What it does:
- Downloads providers
- Configures backend
- Initializes modules
- Creates .terraform/

📌 Must be run first (and after backend/module changes).

```
terraform validate #Syntax errors, Invalid references, Wrong variable types

terraform fmt #Formats Terraform code.
terraform fmt -recursive

terraform plan

desired state (code)
vs
current state (state file)

terraform apply
terraform apply -auto-approve

terraform destroy

terraform output
terraform output vpc_id
terraform output -json

terraform state list
terraform state show aws_instance.web

terraform state rm aws_instance.web
```

### 🤔 What Is terraform import?
- terraform import lets you bring existing infrastructure under Terraform management without recreating it.

### 🧠 When Do You Use Import?
- You use terraform import when:
- Infra was created manually (console / CLI)
- You’re migrating legacy infra to Terraform
- Another tool created the resources
- You want Terraform control without downtime


### Flow:
- 1️⃣ Write resource block (HCL)
- 2️⃣ Run terraform import
- 3️⃣ Terraform updates state only
- 4️⃣ Run terraform plan to reconcile
📌 Import does not create or change infra.
```
terraform import <RESOURCE_ADDRESS> <RESOURCE_ID>
```

### Resource Block
```
resource "aws_instance" "web" {
  # attributes will be filled later
}
```

### Import The resource 
```
terraform import aws_instance.web i-0abcd1234
```
### Terraform:
- Reads EC2 details from AWS
- Stores them in state

```
terraform plan
```
- You’ll see diffs — now update code to match reality.

- “Terraform import allows existing infrastructure to be added to Terraform state so it can be managed without recreation. It’s commonly used during migrations from manually created infrastructure or when recovering state.”

- “terraform state rm removes a resource from Terraform state without deleting the underlying infrastructure. It’s commonly used during refactoring, migrations, or to stop managing a resource, but it must be followed by a plan to avoid accidental recreation.”
  ```
  terraform state pull > backup.tfstate
  ```
  
## 33. Automated Cost Estimation
Leverage Terraform

### 🧱 Terraform Block vs Provider Block

| Block       | Purpose                                                                  |
| ----------- | ------------------------------------------------------------------------ |
| `terraform` | Configures **Terraform itself**                                          |
| `provider`  | Configures **how Terraform talks to a platform** (AWS, Azure, GCP, etc.) |


### Terraform reads:
- 1️⃣ terraform block first
- 2️⃣ provider blocks next
- 3️⃣ Resources after that

```
terraform {
  required_version = ">= 1.5.0"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }

  backend "s3" {
    bucket = "tf-state"
    key    = "dev/app.tfstate"
    region = "ap-south-1"
  }
}

# Default AWS provider (primary region)
provider "aws" {
  region = "ap-south-1"
}

# Aliased AWS provider (secondary region / DR / another account)
provider "aws" {
  alias  = "dr"
  region = "ap-southeast-1"
}

# Uses DEFAULT provider
resource "aws_s3_bucket" "example" {
  bucket = "my-example-bucket-primary"
}

# Uses ALIASED provider
resource "aws_s3_bucket" "example_dr" {
  provider = aws.dr
  bucket   = "my-example-bucket-dr"
}
````
