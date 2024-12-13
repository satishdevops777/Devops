# ANSIBLE

Ansible is a powerful open-source IT automation tool that automates tasks such as configuration management, application deployment, task automation, and multi-node orchestration. 
It is particularly favored for its simplicity and ease of use compared to other tools like Puppet, Chef, or SaltStack.


### 1. What is Ansible?

Ansible is a configuration management and automation tool that helps to manage and configure servers and applications.

It provides a way to automate repetitive tasks, thereby saving time, reducing errors, and ensuring consistency across systems.

It is agentless, meaning it does not require any agent software to be installed on the managed nodes.

### 2. Key Features of Ansible

**Simple:** Ansible uses a simple syntax written in YAML (Yet Another Markup Language), which makes it easy for both beginners and experienced users.

**Agentless:** No agents need to be installed on the remote servers. It relies on SSH (or WinRM for Windows) for communication.

**Idempotent:** Ansible ensures that actions are only performed when necessary, meaning it will not change the state of a system if it's already in the desired state.

**Extensible:** Ansible can be extended with custom modules, roles, and plugins.

**Powerful:** It can handle complex multi-tier deployments, infrastructure management, and cloud provisioning.

### 3. Why Use Ansible?

Ease of Use: Ansible is simple and intuitive. You don’t need to write complex scripts to achieve desired automation.

Scalability: Ansible scales well from a few machines to thousands. It can handle multi-cloud and hybrid cloud environments.

Speed: As a powerful automation tool, Ansible can execute tasks in parallel, making it much faster than manual or script-based processes.

Security: Ansible uses SSH for communication, so there is no need to open additional ports or install agents.

No Need for Agents: Ansible operates without requiring an agent on the managed machines, unlike tools such as Chef or Puppet, which require special agents to be installed.

### 4. How Ansible Works

Ansible operates in a push-based model. You write playbooks (configuration instructions), and Ansible pushes these configurations to the target machines over SSH (or WinRM in the case of Windows). This push model allows for quick and easy automation.

Ansible uses a declarative approach. This means you define the desired end state of a system, and Ansible ensures that the system reaches that state, no matter what the current state is. For example, if you want to ensure a package is installed, Ansible will check if it’s installed and install it if necessary.

### 5. Ansible Architecture

The architecture of Ansible is fairly simple:

Control Node: The machine where Ansible is run from (where the playbooks are executed).

Managed Nodes: The target machines that are being managed by Ansible. These nodes don’t need to have any special agents installed; they only require SSH access (or WinRM for Windows).

Ansible runs on a Control Node and communicates with the Managed Nodes over SSH (for Linux/Unix systems) or WinRM (for Windows systems). Ansible executes commands remotely, applies changes, and ensures systems are in the desired state.

### 6. Ansible Use Cases

Configuration Management: Automating the configuration of systems and applications across multiple servers. This is particularly useful for maintaining consistency in large environments.

Application Deployment: Automating the deployment of applications across environments (development, staging, production).

Provisioning Infrastructure: Using Ansible to provision cloud infrastructure on platforms like AWS, Azure, or Google Cloud.

Continuous Integration/Continuous Deployment (CI/CD): Ansible is widely used in CI/CD pipelines to automate the deployment of software.


## Ansible Variables

Ansible variables are essential for creating dynamic and flexible automation playbooks. 
They allow you to store values that can be used in multiple places within a playbook, reducing redundancy and increasing maintainability. 
Variables can hold a wide range of data, from simple strings to complex data structures, such as dictionaries and lists.

### 1. Basic Ansible Variables
#### 1.1. Defining and Using Variables
Variables can be defined in different places, such as directly in the playbook, in inventory files, or in external files.

**Example: Defining a Variable in a Playbook**

```yaml
---
- name: Basic Ansible Variable Example
  hosts: localhost
  vars:
    my_variable: "Hello, Ansible!"
  tasks:
    - name: Print variable value
      debug:
        msg: "{{ my_variable }}"
```
In this example:

The variable ```my_variable``` is defined under the vars section.

The ```debug``` module is used to print the value of ```my_variable``` using the Jinja2 templating syntax ```{{ my_variable }}```


### 1.2. Variable Scope

Variables in Ansible can be defined at different levels. The scope of the variable determines where it can be accessed.

Global Scope: Variables defined in the vars section of a playbook are available to all tasks in that play.

Host and Group Variables: Variables can be defined per host or group in the inventory.

Task Scope: Variables can also be defined at the task level, limiting their scope to that task.

**Example: Task-Level Variable**

```yaml
---
- name: Task-level variable example
  hosts: localhost
  tasks:
    - name: Define a variable only for this task
      set_fact:
        task_var: "Task-specific value"
    
    - name: Print the task variable
      debug:
        msg: "{{ task_var }}"
```

## 2. Advanced Ansible Variables
### 2.1. Variable Precedence

Ansible resolves variable values by a certain precedence order. This means that when there are multiple definitions of the same variable, Ansible chooses the most specific one.

**Order of Precedence:**

Command line values

Playbook variables

Host variables

Group variables

Defaults in roles

**Example: Precedence of Variables**
Consider a scenario where the same variable is defined at multiple levels:

```yaml
---
- name: Variable Precedence Example
  hosts: localhost
  vars:
    my_var: "Playbook level variable"
  tasks:
    - name: Print the variable
      debug:
        msg: "{{ my_var }}"
```
Now, if you define my_var in the inventory or via the command line (-e flag), the output will depend on the level of precedence.

### 2.2. Dynamic Variables Using Facts

Ansible gathers facts (system information) about hosts before executing tasks. These facts can be used as dynamic variables within the playbook.

**Example: Using Ansible Facts**
```yaml
---
- name: Using Ansible Facts
  hosts: localhost
  tasks:
    - name: Gather facts about the system
      setup:

    - name: Display the gathered fact
      debug:
        msg: "The system's IP address is {{ ansible_default_ipv4.address }}"
```
In this example:

The setup module is used to gather system information.

ansible_default_ipv4.address is a fact variable containing the system’s default IP address.


### 2.3. Jinja2 Templating in Variables

Ansible uses Jinja2 templating to process variables, allowing you to perform operations such as string concatenation, conditionals, and loops.

**Example: Jinja2 String Concatenation**
```yaml
---
- name: Jinja2 Templating Example
  hosts: localhost
  vars:
    greeting: "Hello"
    name: "Ansible"
  tasks:
    - name: Concatenate strings
      debug:
        msg: "{{ greeting }} {{ name }}"
```
**Example: Conditional Statements in Variables**

```yaml
---
- name: Conditional Example
  hosts: localhost
  vars:
    environment: "production"
  tasks:
    - name: Check environment and print message
      debug:
        msg: "{{ 'In Production!' if environment == 'production' else 'Not in Production' }}"
```
## 3. Using Complex Data Types
### 3.1. Lists

Variables can hold lists (arrays), which are often used to define multiple items for a task.

**Example: List Variable**
```yaml
---
- name: List Example
  hosts: localhost
  vars:
    fruits:
      - apple
      - banana
      - cherry
  tasks:
    - name: Print fruits list
      debug:
        msg: "{{ item }}"
      loop: "{{ fruits }}"
```

### 3.2. Dictionaries (Hashmaps)

Variables can also hold dictionaries (key-value pairs). This is useful when you want to define complex data structures.

**Example: Dictionary Variable**
```yaml
- name: Dictionary Example
  hosts: localhost
  vars:
    user_info:
      name: "John Doe"
      age: 30
      city: "New York"
  tasks:
    - name: Print user name
      debug:
        msg: "{{ user_info.name }}"
```

### 3.3. Combining Lists and Dictionaries

You can also combine lists and dictionaries in complex data structures.

**Example: Nested Dictionary with List**
```yaml
---
- name: Nested Data Example
  hosts: localhost
  vars:
    people:
      - name: "Alice"
        age: 28
        city: "Chicago"
      - name: "Bob"
        age: 35
        city: "San Francisco"
  tasks:
    - name: Print name and city for each person
      debug:
        msg: "{{ item.name }} lives in {{ item.city }}"
      loop: "{{ people }}"
```

## 4. Working with External Variable Files

You can separate variables into different files for better organization and reuse.

### 4.1. Variable Files

Variables can be stored in separate YAML files and included in the playbook.

**Example: Using a Variable File**

``vars.yml``
```
username: "admin"
password: "secretpassword"
```

``playbook.yml``

```yaml
---
- name: Include Variable File Example
  hosts: localhost
  vars_files:
    - vars.yml
  tasks:
    - name: Print username and password
      debug:
        msg: "Username: {{ username }}, Password: {{ password }}"
```

## 5. Advanced Features
### 5.1. Looping with Indexes

You can loop through lists or dictionaries and get access to the index or key-value pairs.

**Example: Looping with Index**

```yaml
---
- name: Looping with Index Example
  hosts: localhost
  vars:
    names:
      - Alice
      - Bob
      - Carol
  tasks:
    - name: Print name and index
      debug:
        msg: "Index: {{ loop.index }}, Name: {{ item }}"
      loop: "{{ names }}"
```

### 5.2. Using set_fact to Define Variables Dynamically

You can dynamically create or change variables during playbook execution using the set_fact module.

**Example: Using set_fact to Change Variable**
```yaml
---
- name: Set Fact Example
  hosts: localhost
  tasks:
    - name: Set a dynamic fact
      set_fact:
        dynamic_var: "{{ 10 + 5 }}"
    
    - name: Print dynamic fact
      debug:
        msg: "The dynamic variable is {{ dynamic_var }}"

```
