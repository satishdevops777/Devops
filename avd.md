# Devops project

## How to increase ec2 instance volume?
***1. Check disk & partition***
```bash
lsblk

Example output:
nvme0n1       50G
└─nvme0n1p1   20G
```
- This means you extended disk to 50GB but partition is still 20GB.

***2. Grow the partition***
```bash
sudo growpart /dev/nvme0n1 1
```
***3. Resize filesystem***
```bash
sudo resize2fs /dev/nvme0n1p1
```
***4. Verify***
```bash
df -h
```

## What is a dependency file?
- A dependency file is a file used by a programming language or a package manager to list the external libraries, modules, or packages that your project needs in order to run or declaration file that lists all dependencies so that package managers know what to install.
- It tells the system:
  - What dependencies your project uses
  - Which version of each dependency
  - Sometimes: build instructions, scripts, environment settings, etc.
  
  ***Example with Python (requirements.txt)***
```ini
flask==3.0.0
sqlalchemy==2.0.0
requests==2.31.0
```
***Example with Node.js (package.json)***
```json
{
  "dependencies": {
    "express": "^4.18.2",
    "mongoose": "^7.0.0"
  }
}
```
***Example with Java (Maven — pom.xml)***
```xml
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-core</artifactId>
  <version>5.3.2</version>
</dependency>
```
***Example with Go (go.mod)***
```bash
module example.com/myapp
go 1.22
require github.com/gin-gonic/gin v1.9.1
```
***⚡ Do all programming languages have a dependency file?***
- Not every language — but every modern project ecosystem typically uses one.
- Languages with dependency files:
| Language                 | Dependency File                          |
| ------------------------ | ---------------------------------------- |
| **Python**               | requirements.txt, pyproject.toml         |
| **Node.js / JavaScript** | package.json                             |
| **Go**                   | go.mod, go.sum                           |
| **Java**                 | pom.xml (Maven), build.gradle (Gradle)   |
| **Ruby**                 | Gemfile                                  |
| **Rust**                 | Cargo.toml                               |
| **PHP**                  | composer.json                            |
| **C# (.NET)**            | .csproj                                  |
| **Terraform**            | .tf + versions.tf (providers + versions) |

***🎯 Why dependency files are important?***
- Ensure consistent versions across environments
- Avoid “it works on my machine” issues
- Allow reproducible builds
- Simplify installing everything (e.g., pip install -r requirements.txt)
- Essential for DevOps, CI/CD, SRE workflows

***✅ What is a Dependency?***
- A dependency is any external code your project relies on.
- Example:
  - A Python project might depend on Flask.
  - A Go project might depend on a Prometheus exporter library.
  - A Node.js project might depend on Express.js.
  - A Java project might depend on Spring Boot.
 
# Package Manager
- A package manager is a tool that automatically downloads, installs, updates, and manages software libraries (packages) that your project depends on.
- Think of it like an app store for programming libraries.
***🧩 Simple Definition***
- A package manager is a tool that:
- Provides ready-made libraries you can use
- Installs them automatically
- Tracks versions
- Updates them safely
- Resolves dependency conflicts
- Ensures your project builds the same way everywhere

***🔧 Examples of Package Managers (Language-wise)***
| Language / Ecosystem | Package Manager | Dependency File                   |
| -------------------- | --------------- | --------------------------------- |
| **Python**           | pip / pipenv    | requirements.txt / pyproject.toml |
| **Node.js / JS**     | npm / yarn      | package.json                      |
| **Go**               | go mod          | go.mod                            |
| **Java**             | Maven / Gradle  | pom.xml / build.gradle            |
| **Ruby**             | Bundler         | Gemfile                           |
| **PHP**              | Composer        | composer.json                     |
| **Rust**             | Cargo           | Cargo.toml                        |
| **Linux**            | apt / yum / dnf | (system-level pkgs)               |

