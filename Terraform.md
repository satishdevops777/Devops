
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

<img width="391" height="685" alt="image" src="https://github.com/user-attachments/assets/0aa10bb4-18c1-4448-a8f6-00a5feb22035" />

---

## 2. Data Types
Supported data types in Terraform:
- **string**: A sequence of Unicode characters (e.g., `"hello"`).
- **number**: A numeric value.
- **bool**: True or false.
- **list**: A sequence of values (e.g., `["a", 15, true]`).
- **set**: A collection of unique values with no secondary identifiers.
- **map**: A group of values identified by named labels (e.g., `{name="mabel", age=52}`).
- **null**: Represents absence or omission.

---

## 3. Count and `count.index`
- Use `count` and `count.index` to create identical resources with different names.
- Syntax: `[count.index]`.

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

---

## 8. Terraform Format
- Use `terraform fmt` to ensure consistent formatting of configuration files.

---

## 9. Data Sources
Fetch information defined outside Terraform.
Example:
```hcl
data "local_file" "food" {
  filename = "${path.module}/filename"
}
```

---

## 10. Terraform Graph
Generate a visual representation of dependencies.
```bash
terraform graph -type=plan | dot -Tpng > graph.png
```

---

## 11. Security Primer
- **Sensitive Parameters:** Prevent data exposure in logs or CLI.
- **HashiCorp Vault:** Securely store secrets.
- **Terraform.lock.hcl:** Manages provider plugins and Terraform versions.

---

## 12. Terraform Cloud
- Manage runs in a consistent environment.
- Features: Access control, private registry, policy controls.
- **Sentinel:** Policy as code framework.
- **Remote Backend:** Includes Terraform Cloud, Local, and Remote backends.

---

## 13. Load Order & Semantics
- Terraform loads files alphabetically.
- Extensions must be `.tf` or `.tf.json`.

---

## 14. Terraform Validate
- Run `terraform validate` to ensure syntactic correctness.

---

## 15. Resource Behavior and Meta Arguments
Customize resource behavior using meta-arguments:
- **depends_on**: Handle hidden dependencies.
- **count**: Create multiple resource instances.
- **for_each**: Create resources for each map or set item.
- **lifecycle**:
  - `create_before_destroy`: Create a new resource before destroying the old one.
  - `prevent_destroy`: Prevent accidental destruction.
  - `ignore_changes`: Ignore specific changes.
  - `replace_triggered_by`: Replace resource when dependencies change.

---

## 16. Terraform Taint
Force resource replacement on the next apply.
```bash
terraform apply -replace="resourcename.localname"
```

---

## 17. Terraform Modules
Enable reusable configurations with a standard structure:
- **main.tf**: Resource logic.
- **variables.tf**: Input variables.
- **outputs.tf**: Output values.

Example:
```hcl
module "eks" {
  source  = "terraform-aws-modules/eks/aws"
  version = "20.11.1"
}
```

---

## 18. Terraform Workspaces
Manage multiple environments with the same configuration.
```bash
terraform workspace new dev
terraform workspace select dev
```

---

## 19. Remote State Management
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
Deploy resources across multiple cloud providers in the same configuration.

---

## 29. Resource Targeting
Apply changes only to specific resources without affecting others:
```bash
terraform apply -target=aws_instance.example
```

---

## 30. Complex Outputs
Export structured data like lists or maps.
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

## 33. Automated Cost Estimation
Leverage Terraform
