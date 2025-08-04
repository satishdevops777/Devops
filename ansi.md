# Ansible - Complete Guide

Ansible is an open-source automation tool that helps in configuration management, application deployment, task automation, and orchestration.

---

## Key Features

* **Agentless**: Uses SSH or WinRM for communication, no agents needed on managed nodes.
* **Simple Syntax**: Employs YAML for human-readable playbooks.
* **Idempotent**: Ensures tasks only make changes when necessary, preventing unintended side effects.
* **Powerful & Scalable**: Suitable for everything from small personal setups to large enterprise deployments.

---

## 1. Ansible Core Concepts

### 1.1 Automation Areas

* **Provisioning**: Setting up servers or cloud resources (e.g., launching AWS EC2 instances).
* **Configuration Management**: Ensuring servers are configured to a defined state (e.g., NGINX installed, SSH configured).
* **Continuous Delivery**: Automating builds and deployments via pipelines.
* **App Deployment**: Rolling out applications and dependencies across environments.
* **Security Compliance**: Enforcing baseline OS-level security (e.g., disable root SSH, set correct permissions).

### Example: Provisioning a Web Server

```yaml
- name: Provision NGINX on Ubuntu
  hosts: webservers
  become: true
  tasks:
    - name: Install NGINX
      apt:
        name: nginx
        state: present
```

## 2. Configuration Files

Ansible's behavior is controlled by configuration files.

**Default Config File**
`/etc/ansible/ansible.cfg`

**Precedence Order**
Ansible reads configuration settings in a specific order, with later settings overriding earlier ones:

1. `$ANSIBLE_CONFIG=/path/to/file` – Environment variable
2. `./ansible.cfg` – Local directory
3. `~/.ansible.cfg` – User home directory
4. `/etc/ansible/ansible.cfg` – Default system-wide config

**Useful Commands**

```bash
ansible-config list   # Lists all Ansible configuration options with details
ansible-config view   # Shows the config file Ansible is currently using
ansible-config dump   # Dumps the actual values used by Ansible, including overrides
```

**Example:** Set More Parallel Connections (forks)

**INI or TOML Format:**

```ini
[defaults]
forks = 10
host_key_checking = False
retry_files_enabled = False
```

**Explanation**

- `forks`: Determines how many hosts Ansible can connect to in parallel.
- `host_key_checking`: Disables SSH prompt on first connection.
- `retry_files_enabled`: Disables creation of `.retry` files.

## 3. YAML (Yet Another Markup Language)

YAML is fundamental to Ansible, used for writing playbooks, inventories, and variable files.

**Syntax Example**

```yaml
Fruit: Apple  # Key-Value
Vegetable: Carrot
liquid: Water
Meat: Chicken
```
**List Example**
```yaml
fruits:
  - Apple
  - Banana
  - Mango
```
💡 Tip: Always use 2 spaces for indentation. Do not use tabs.

## 4. Ansible Inventory
Inventory defines the systems managed by Ansible.

 **INI Format**
```ini
[webservers]
web1.example.com ansible_user=ubuntu
web2.example.com ansible_user=ubuntu

[dbservers]
db1.example.com ansible_user=root
```
**YAML Format**
```yaml
all:
  children:
    webservers:
      hosts:
        web1.example.com:
        web2.example.com:
    dbservers:
      hosts:
        db1.example.com:
```
**SSH Login Example**
```ini
[webservers]
10.0.1.10 ansible_user=ec2-user ansible_ssh_private_key_file=~/.ssh/aws.pem\
```
**Command Example**
```bash
ansible webservers -m ping
```
**-m ping:** Uses the ping module to check connectivity between Ansible and the target hosts.

## 5. Ansible Variables (with Examples)
Variables make playbooks dynamic and reusable.

**Defining Variables in Playbook** 
```YAML
- name: Print a message
  hosts: localhost
  vars:
    greeting: "Hello from Ansible!"
  tasks:
    - name: Display variable
      debug:
        msg: "{{ greeting }}"
```
**Defining Variables in Inventory**
```Ini
[webservers]
web1 ansible_host=192.168.1.100 app_port=8080
```
**Defining in Group Vars**
```Ini
[webservers:vars]
app_name=myapp
```
**Accessing in Playbook:**
```YAML
msg: "App name is {{ app_name }} and runs on port {{ app_port }}"
```
**Boolean Example**
```YAML
vars:
  is_enabled: true
```
**List and Dictionary**
```YAML
vars:
  packages:
    - nginx
    - git
  user:
    name: devops
    shell: /bin/bash
```
**Using Variables in a Loop**
```YAML
- name: Install packages
  apt:
    name: "{{ item }}"
    state: present
  loop: "{{ packages }}"
```
## 6. Registering Variables and Variable Precedence
### Register Output of a Task
You can capture the result of a task using register and use it later.

**Example:**

```YAML
- name: Get the uptime of the system
  command: uptime
  register: uptime_result

- name: Print result
  debug:
    var: uptime_result.stdout # Displays the uptime output
```
**Variable Precedence (from highest to lowest)**
1. Extra vars (ansible-playbook site.yml -e "var=value")
2. Set facts
3. Include vars (via include_vars)
4. Role vars (defined within a role)
5. Play vars (inside the playbook vars: section)
6. Host facts (discovered via setup)
7. Host vars (defined in host_vars/ or inventory)
8. Group vars (defined in group_vars/ or inventory)
9. Role defaults (lowest priority)

**Example:**

```Bash
ansible-playbook playbook.yml -e "env=production"
```
This overrides any env variable defined elsewhere.

**Variable Defined in Inventory**
```Ini
[webservers:vars]
dns_server=10.5.5.3

[webservers]
web1 ansible_host=172.20.1.100 dns_server=10.5.5.5
```
In this case, web1 uses 10.5.5.5 because the host-level variable overrides the group-level.

## 7. Magic Variables and Scopes
Magic variables are built-in Ansible variables that provide information about hosts, groups, and inventory structure.

**Hostvars (Access data from other hosts)**
```YAML

- name: Print DNS of another host
  debug:
    msg: "DNS of web2: {{ hostvars['web2'].dns_server }}"
```
hostvars is a dictionary of host-specific variables.

**Groups and Inventory**
```YAML

- name: Print all hosts in 'webservers'
  debug:
    var: groups['webservers']

- name: Print the groups of current host
  debug:
    var: group_names

- name: Print the name of the host
  debug:
    var: inventory_hostname
```
**Facts from Another Host**
```YAML
- name: Print CPU info from web2
  debug:
    msg: "CPU: {{ hostvars['web2']['ansible_facts']['processor'][0] }}"
```
**Scope Types:**
**Host Scope:** Variables tied to a specific host.
**Play Scope:** Variables available only during a play.

## 8. Ansible Facts
Facts are system data (hostname, IP, OS, etc.) gathered by Ansible from managed nodes.

**Enable Fact Gathering (default)**
```YAML

gather_facts: true
```
**Disable Fact Gathering**
```YAML
gather_facts: false
```
**Accessing Facts**
```YAML
- name: Show default IP
  debug:
    msg: "Default IPv4: {{ ansible_default_ipv4.address }}"
```
### Gathering Modes (via config or ansible.cfg)
**implicit:** Default, facts gathered unless turned off.
**explicit:** Gather only when you write gather_facts: true.
**smart:** Facts are reused unless missing.

**Command Line:**
```Bash
ansible localhost -m setup # Gather and print all facts
```
## 9. Ansible Playbooks
Playbooks are YAML files that define sets of instructions for Ansible to follow. They allow you to orchestrate multiple tasks, control configuration, deploy applications, and much more.

### Playbook Structure:
**Play:** Applies tasks to a group of hosts.
**Task:** Executes a single module on the host.
**Module:** The unit of work (like yum, apt, copy, etc.).
**Handlers:** Triggered only when notified by a task.
**Variables:** Provide dynamic input to tasks.

**Example:**
```YAML
- name: Install and configure NGINX
  hosts: webservers
  become: true
  vars:
    nginx_port: 80
  tasks:
    - name: Install NGINX
      apt:
        name: nginx
        state: present

    - name: Ensure NGINX is running
      service:
        name: nginx
        state: started
        enabled: true

    - name: Open firewall port
      ufw:
        rule: allow
        port: "{{ nginx_port }}"
        proto: tcp
```
This playbook installs NGINX, starts it, enables it on boot, and opens the port.

## 10. Modules
Modules are the actual units of work in Ansible. Each task in a playbook invokes a module.

### Commonly Used Modules:
**System:** user, group, service, hostname
**Package:** apt, yum, dnf, pip
**File:** copy, template, file, lineinfile
**Command:** command, shell, script, raw

**Example:**  File Module
```YAML
- name: Create a directory
  file:
    path: /opt/myapp
    state: directory
    owner: ubuntu
    mode: '0755'
```
**Example:**  Copy Module
```YAML
- name: Copy config file
  copy:
    src: nginx.conf
    dest: /etc/nginx/nginx.conf
```
**Example:** Template Module (with Jinja2)
```YAML
- name: Deploy nginx config
  template:
    src: templates/nginx.conf.j2
    dest: /etc/nginx/nginx.conf
```
Templates allow variable substitution using Jinja2 syntax:
**Nginx**
```yaml
server {
    listen {{ nginx_port }};
    server_name {{ server_name }};
}
```

## 11. Playbook Execution Modes
These modes help you test your playbooks safely before applying changes.

**Check Mode (Dry Run)**
```Bash
ansible-playbook site.yml --check
```
This simulates the execution and shows what changes would happen without making any actual changes.

**Diff Mode (Show Before/After Changes)**
```Bash
ansible-playbook site.yml --diff
```
Useful for file-based tasks (e.g., template, lineinfile) to show differences.

**Syntax Check**
```Bash
ansible-playbook site.yml --syntax-check
```
Validates the YAML structure of your playbook.

## 12. Loops & Conditionals
**Simple Loop (List)**
```YAML
- name: Install packages
  apt:
    name: "{{ item }}"
    state: present
  loop:
    - git
    - curl
    - htop
```
**Loop with Dictionary**
```YAML
- name: Create users
  user:
    name: "{{ item.name }}"
    uid: "{{ item.uid }}"
  loop:
    - { name: alice, uid: 1001 }
    - { name: bob, uid: 1002 }
```
**Legacy Loop with with_items**
```YAML
- name: Install packages (legacy)
  apt:
    name: "{{ item }}"
    state: present
  with_items:
    - git
    - curl
```
**Using Conditions**
```YAML
- name: Restart service if enabled
  service:
    name: apache2
    state: restarted
  when: is_enabled
```
**Example:** Only install if on Debian:
```YAML
- name: Install Apache
  apt:
    name: apache2
    state: present
  when: ansible_os_family == 'Debian'
```
## 13. Handlers
Handlers are tasks that run only when notified. They’re typically used for restarting services after a configuration change.
**Example:**
```YAML
- name: Install NGINX
  apt:
    name: nginx
    state: present
  notify: restart nginx
handlers:
  - name: restart nginx
    service:
      name: nginx
      state: restarted
```
**When is a handler triggered?**
Handlers are triggered only if the task using notify reports a change. If the task does not change the system state, the handler is skipped.

## 14. Roles
Roles are structured directories that allow easy reuse and sharing of automation content.

### Role Directory Layout:
```yaml
roles/
  apache/
    tasks/
      main.yml
    handlers/
      main.yml
    templates/
      httpd.conf.j2
    vars/
      main.yml
    defaults/
      main.yml
```
**Create a Role:**
```Bash
ansible-galaxy init apache
```
**Use a Role in Playbook:**
```YAML
- name: Deploy web server
  hosts: webservers
  roles:
    - apache
```
**Install a Public Role:**
```Bash
ansible-galaxy install geerlingguy.apache
```
## 15. Collections
Collections are packages that bundle modules, roles, and plugins together.

**Install a Collection:**
```Bash
ansible-galaxy collection install community.general
```
**Use Modules from a Collection:**
```YAML
- name: Use sysctl from community.general
  community.general.sysctl:
    name: net.ipv4.ip_forward
    value: '1'
    state: present
```
## 16. Templating with Jinja2
Templating in Ansible is done using the Jinja2 engine. You can embed variables and expressions inside templates, which are typically used with the template module.

**Template File Example (nginx.conf.j2):**

```
# Nginx
server {
    listen {{ nginx_port }};
    server_name {{ server_name }};
    root {{ document_root }};
}
```
**Template Task in Playbook:**
```YAML

- name: Deploy NGINX config
  template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
```
**Common Filters in Jinja2:**
```
{{ user_name | upper }}             # Converts to uppercase
{{ list_of_users | join(', ') }}    # Joins list with comma
{{ 3.14159 | round(2) }}            # Rounds to 2 decimal places
```
## 17. Environment Setup (Local Dev/Test)
You can simulate Ansible environments using tools like:

**1. Vagrant**
Used to create reproducible local environments.
```Bash
vagrant init ubuntu/bionic64
vagrant up
```
**2. Docker**
Run containers for testing roles or modules.

```Bash
docker run -it ubuntu:latest bash
```
**3. VMware/VirtualBox**
Useful for simulating real infrastructure with virtual machines.

## 18. Variable File Separation
You can store variables in separate files for better organization.

**Using vars_files:**
```YAML
- name: Example with vars file
  hosts: localhost
  vars_files:
    - vars/common.yml
  tasks:
    - name: Print custom message
      debug:
        msg: "{{ welcome_message }}"
```
**Using host_vars and group_vars:**
Create directories named host_vars/ and group_vars/ in the same directory as your playbook.

Add YAML files with the hostname or group name inside these directories.

**Example:** host_vars/web1.yml

```YAML
nginx_port: 80
server_name: web1.example.com
```
## 19. Async Actions
Some tasks take a long time to run (e.g., backups, updates). You can run them asynchronously without blocking.

**Example:**
```YAML
- name: Run a long shell script asynchronously
  command: /opt/long_script.sh
  async: 600   # Max seconds to run
  poll: 0      # Fire and forget (don't wait for completion)
  register: job_result

- name: Check async job status
  async_status:
    jid: "{{ job_result.ansible_job_id }}"
  register: job_status
  until: job_status.finished
  retries: 5
  delay: 10
```
## 20. Strategy
Defines how Ansible runs tasks across hosts.

**Linear Strategy (default)**
```YAML
strategy: linear
```
Runs tasks on one host at a time, in order.

**Free Strategy**
```YAML
strategy: free #Runs tasks on all hosts as soon as they’re ready.
```
**Serial (batch execution)**
```YAML
serial: 3 #Run 3 hosts at a time.
```

## 21. Error Handling
You can control task behavior on failure.

**Example:** Ignore error
```YAML
- name: Try stopping a service
  service:
    name: fake-service
    state: stopped
  ignore_errors: yes
```
**Fail only if custom condition is true**
```YAML
- name: Conditional failure
  command: some_command
  register: cmd_out
  failed_when: cmd_out.rc != 0
```
**Stop all on error**
```YAML
any_errors_fatal: true
```
## 22. Ansible Vault
Encrypt secrets like API keys, passwords, and certificates.

**Encrypt a file:**
```Bash
ansible-vault encrypt secrets.yml
```
**Decrypt/view/edit:**
```Bash
ansible-vault decrypt secrets.yml
ansible-vault view secrets.yml
ansible-vault edit secrets.yml
```
**Use in playbook:**
```Bash
ansible-playbook site.yml --ask-vault-pass
ansible-playbook site.yml --vault-password-file ~/.vault_pass.txt
```
## 23. Plugins
Plugins extend Ansible’s core functionality.

### Types:
**Callback:** Format output (e.g., json, yaml, minimal).
**Connection:** SSH, WinRM, local.
**Lookup:** Read external data (e.g., CSV, files).
**Filter:** Custom Jinja2 filters.

**Set custom plugin paths:**
```Bash
export ANSIBLE_FILTER_PLUGINS=/custom/filter_plugins
export ANSIBLE_STDOUT_CALLBACK=json
```
**Lookup Example:**
```YAML
- name: Read a value from CSV
  debug:
    msg: "{{ lookup('csvfile', 'user1 file=users.csv delimiter=,') }}"
```
## 24. Tags
Tags allow you to run part of a playbook.

**Tag a task:**
```YAML
- name: Install NGINX
  apt:
    name: nginx
    state: present
  tags:
    - web
    - nginx
```
**Run only tagged tasks:**
```Bash
ansible-playbook web.yml --tags nginx
```
**Skip tagged tasks:**
```Bash
ansible-playbook web.yml --skip-tags debug
```






















Markdown

# Ansible - Complete Guide

Ansible is an open-source automation tool that helps in configuration management, application deployment, task automation, and orchestration.

---

## Key Features

* **Agentless**: Uses SSH or WinRM for communication, no agents needed on managed nodes.
* **Simple Syntax**: Employs YAML for human-readable playbooks.
* **Idempotent**: Ensures tasks only make changes when necessary, preventing unintended side effects.
* **Powerful & Scalable**: Suitable for everything from small personal setups to large enterprise deployments.

---

## 1. Ansible Core Concepts

### 1.1 Automation Areas

* **Provisioning**: Setting up servers or cloud resources (e.g., launching AWS EC2 instances).
* **Configuration Management**: Ensuring servers are configured to a defined state (e.g., NGINX installed, SSH configured).
* **Continuous Delivery**: Automating builds and deployments via pipelines.
* **App Deployment**: Rolling out applications and dependencies across environments.
* **Security Compliance**: Enforcing baseline OS-level security (e.g., disable root SSH, set correct permissions).

### Example: Provisioning a Web Server

```yaml
- name: Provision NGINX on Ubuntu
  hosts: webservers
  become: true
  tasks:
    - name: Install NGINX
      apt:
        name: nginx
        state: present
2. Configuration Files
Ansible's behavior is controlled by configuration files.

Default Config File: /etc/ansible/ansible.cfg

Precedence Order:
Ansible reads configuration settings in a specific order, with later settings overriding earlier ones:

$ANSIBLE_CONFIG=/path/to/file – Environment variable.

./ansible.cfg – Local directory.

~/.ansible.cfg – User home directory.

/etc/ansible/ansible.cfg – Default system-wide config.

Useful Commands:
ansible-config list : Lists all Ansible configuration options with details.

ansible-config view : Shows the config file Ansible is currently using.

ansible-config dump : Dumps the actual values used by Ansible, including overrides.

Example: Set More Parallel Connections (forks)
Ini, TOML

[defaults]
forks = 10
host_key_checking = False
retry_files_enabled = False
Explanation:

forks determines how many hosts Ansible can connect to in parallel.

host_key_checking disables SSH prompt on first connection.

retry_files_enabled disables creation of .retry files.

3. YAML (Yet Another Markup Language)
YAML is fundamental to Ansible, used for writing playbooks, inventories, and variable files.

Syntax Example:
YAML

Fruit: Apple # Key-Value
Vegetable: Carrot
liquid: Water
Meat: Chicken
List Example:
YAML

fruits:
  - Apple
  - Banana
  - Mango
Tip: Always use 2 spaces for indentation. No tabs.

4. Ansible Inventory
Inventory defines the systems managed by Ansible.

INI Format:
Ini, TOML

[webservers]
web1.example.com ansible_user=ubuntu
web2.example.com ansible_user=ubuntu

[dbservers]
db1.example.com ansible_user=root
YAML Format:
YAML

all:
  children:
    webservers:
      hosts:
        web1.example.com:
        web2.example.com:
    dbservers:
      hosts:
        db1.example.com:
SSH Login Example:
Ini, TOML

[webservers]
10.0.1.10 ansible_user=ec2-user ansible_ssh_private_key_file=~/.ssh/aws.pem
Command Example:
Bash

ansible webservers -m ping
-m ping: Use the ping module to check connectivity.

5. Ansible Variables (with Examples)
Variables make playbooks dynamic and reusable.

Defining Variables in Playbook
YAML

- name: Print a message
  hosts: localhost
  vars:
    greeting: "Hello from Ansible!"
  tasks:
    - name: Display variable
      debug:
        msg: "{{ greeting }}"
Defining Variables in Inventory
Ini, TOML

[webservers]
web1 ansible_host=192.168.1.100 app_port=8080
Defining in Group Vars
Ini, TOML

[webservers:vars]
app_name=myapp
Accessing in Playbook:
YAML

msg: "App name is {{ app_name }} and runs on port {{ app_port }}"
Boolean Example
YAML

vars:
  is_enabled: true
List and Dictionary
YAML

vars:
  packages:
    - nginx
    - git
  user:
    name: devops
    shell: /bin/bash
Using Variables in a Loop
YAML

- name: Install packages
  apt:
    name: "{{ item }}"
    state: present
  loop: "{{ packages }}"
6. Registering Variables and Variable Precedence
Register Output of a Task
You can capture the result of a task using register and use it later.

Example:

YAML

- name: Get the uptime of the system
  command: uptime
  register: uptime_result

- name: Print result
  debug:
    var: uptime_result.stdout # Displays the uptime output
Variable Precedence (from highest to lowest)
Extra vars (ansible-playbook site.yml -e "var=value")

Set facts

Include vars (via include_vars)

Role vars (defined within a role)

Play vars (inside the playbook vars: section)

Host facts (discovered via setup)

Host vars (defined in host_vars/ or inventory)

Group vars (defined in group_vars/ or inventory)

Role defaults (lowest priority)

Example:

Bash

ansible-playbook playbook.yml -e "env=production"
This overrides any env variable defined elsewhere.

Variable Defined in Inventory
Ini, TOML

[webservers:vars]
dns_server=10.5.5.3

[webservers]
web1 ansible_host=172.20.1.100 dns_server=10.5.5.5
In this case, web1 uses 10.5.5.5 because the host-level variable overrides the group-level.

7. Magic Variables and Scopes
Magic variables are built-in Ansible variables that provide information about hosts, groups, and inventory structure.

Hostvars (Access data from other hosts)
YAML

- name: Print DNS of another host
  debug:
    msg: "DNS of web2: {{ hostvars['web2'].dns_server }}"
hostvars is a dictionary of host-specific variables.

Groups and Inventory
YAML

- name: Print all hosts in 'webservers'
  debug:
    var: groups['webservers']

- name: Print the groups of current host
  debug:
    var: group_names

- name: Print the name of the host
  debug:
    var: inventory_hostname
Facts from Another Host
YAML

- name: Print CPU info from web2
  debug:
    msg: "CPU: {{ hostvars['web2']['ansible_facts']['processor'][0] }}"
Scope Types:
Host Scope: Variables tied to a specific host.

Play Scope: Variables available only during a play.

8. Ansible Facts
Facts are system data (hostname, IP, OS, etc.) gathered by Ansible from managed nodes.

Enable Fact Gathering (default)
YAML

gather_facts: true
Disable Fact Gathering
YAML

gather_facts: false
Accessing Facts
YAML

- name: Show default IP
  debug:
    msg: "Default IPv4: {{ ansible_default_ipv4.address }}"
Gathering Modes (via config or ansible.cfg)
implicit: Default, facts gathered unless turned off.

explicit: Gather only when you write gather_facts: true.

smart: Facts are reused unless missing.

Command Line:
Bash

ansible localhost -m setup # Gather and print all facts
9. Ansible Playbooks
Playbooks are YAML files that define sets of instructions for Ansible to follow. They allow you to orchestrate multiple tasks, control configuration, deploy applications, and much more.

Playbook Structure:
Play: Applies tasks to a group of hosts.

Task: Executes a single module on the host.

Module: The unit of work (like yum, apt, copy, etc.).

Handlers: Triggered only when notified by a task.

Variables: Provide dynamic input to tasks.

Example:
YAML

- name: Install and configure NGINX
  hosts: webservers
  become: true
  vars:
    nginx_port: 80
  tasks:
    - name: Install NGINX
      apt:
        name: nginx
        state: present

    - name: Ensure NGINX is running
      service:
        name: nginx
        state: started
        enabled: true

    - name: Open firewall port
      ufw:
        rule: allow
        port: "{{ nginx_port }}"
        proto: tcp
This playbook installs NGINX, starts it, enables it on boot, and opens the port.

10. Modules
Modules are the actual units of work in Ansible. Each task in a playbook invokes a module.

Commonly Used Modules:
System: user, group, service, hostname

Package: apt, yum, dnf, pip

File: copy, template, file, lineinfile

Command: command, shell, script, raw

Example: File Module
YAML

- name: Create a directory
  file:
    path: /opt/myapp
    state: directory
    owner: ubuntu
    mode: '0755'
Example: Copy Module
YAML

- name: Copy config file
  copy:
    src: nginx.conf
    dest: /etc/nginx/nginx.conf
Example: Template Module (with Jinja2)
YAML

- name: Deploy nginx config
  template:
    src: templates/nginx.conf.j2
    dest: /etc/nginx/nginx.conf
Templates allow variable substitution using Jinja2 syntax:

Nginx

server {
    listen {{ nginx_port }};
    server_name {{ server_name }};
}
11. Playbook Execution Modes
These modes help you test your playbooks safely before applying changes.

Check Mode (Dry Run)
Bash

ansible-playbook site.yml --check
This simulates the execution and shows what changes would happen without making any actual changes.

Diff Mode (Show Before/After Changes)
Bash

ansible-playbook site.yml --diff
Useful for file-based tasks (e.g., template, lineinfile) to show differences.

Syntax Check
Bash

ansible-playbook site.yml --syntax-check
Validates the YAML structure of your playbook.

12. Loops & Conditionals
Simple Loop (List)
YAML

- name: Install packages
  apt:
    name: "{{ item }}"
    state: present
  loop:
    - git
    - curl
    - htop
Loop with Dictionary
YAML

- name: Create users
  user:
    name: "{{ item.name }}"
    uid: "{{ item.uid }}"
  loop:
    - { name: alice, uid: 1001 }
    - { name: bob, uid: 1002 }
Legacy Loop with with_items
YAML

- name: Install packages (legacy)
  apt:
    name: "{{ item }}"
    state: present
  with_items:
    - git
    - curl
Using Conditions
YAML

- name: Restart service if enabled
  service:
    name: apache2
    state: restarted
  when: is_enabled
Example: Only install if on Debian:
YAML

- name: Install Apache
  apt:
    name: apache2
    state: present
  when: ansible_os_family == 'Debian'
13. Handlers
Handlers are tasks that run only when notified. They’re typically used for restarting services after a configuration change.

Example:
YAML

- name: Install NGINX
  apt:
    name: nginx
    state: present
  notify: restart nginx

handlers:
  - name: restart nginx
    service:
      name: nginx
      state: restarted
When is a handler triggered?
Handlers are triggered only if the task using notify reports a change. If the task does not change the system state, the handler is skipped.

14. Roles
Roles are structured directories that allow easy reuse and sharing of automation content.

Role Directory Layout:
roles/
  apache/
    tasks/
      main.yml
    handlers/
      main.yml
    templates/
      httpd.conf.j2
    vars/
      main.yml
    defaults/
      main.yml
Create a Role:
Bash

ansible-galaxy init apache
Use a Role in Playbook:
YAML

- name: Deploy web server
  hosts: webservers
  roles:
    - apache
Install a Public Role:
Bash

ansible-galaxy install geerlingguy.apache
15. Collections
Collections are packages that bundle modules, roles, and plugins together.

Install a Collection:
Bash

ansible-galaxy collection install community.general
Use Modules from a Collection:
YAML

- name: Use sysctl from community.general
  community.general.sysctl:
    name: net.ipv4.ip_forward
    value: '1'
    state: present
16. Templating with Jinja2
Templating in Ansible is done using the Jinja2 engine. You can embed variables and expressions inside templates, which are typically used with the template module.

Template File Example (nginx.conf.j2):
Nginx

server {
    listen {{ nginx_port }};
    server_name {{ server_name }};
    root {{ document_root }};
}
Template Task in Playbook:
YAML

- name: Deploy NGINX config
  template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
Common Filters in Jinja2:
Code snippet

{{ user_name | upper }}             # Converts to uppercase
{{ list_of_users | join(', ') }}    # Joins list with comma
{{ 3.14159 | round(2) }}            # Rounds to 2 decimal places
17. Environment Setup (Local Dev/Test)
You can simulate Ansible environments using tools like:

1. Vagrant
Used to create reproducible local environments.

Bash

vagrant init ubuntu/bionic64
vagrant up
2. Docker
Run containers for testing roles or modules.

Bash

docker run -it ubuntu:latest bash
3. VMware/VirtualBox
Useful for simulating real infrastructure with virtual machines.

18. Variable File Separation
You can store variables in separate files for better organization.

Using vars_files:
YAML

- name: Example with vars file
  hosts: localhost
  vars_files:
    - vars/common.yml
  tasks:
    - name: Print custom message
      debug:
        msg: "{{ welcome_message }}"
Using host_vars and group_vars:
Create directories named host_vars/ and group_vars/ in the same directory as your playbook.

Add YAML files with the hostname or group name inside these directories.

Example: host_vars/web1.yml

YAML

nginx_port: 80
server_name: web1.example.com
19. Async Actions
Some tasks take a long time to run (e.g., backups, updates). You can run them asynchronously without blocking.

Example:
YAML

- name: Run a long shell script asynchronously
  command: /opt/long_script.sh
  async: 600   # Max seconds to run
  poll: 0      # Fire and forget (don't wait for completion)
  register: job_result

- name: Check async job status
  async_status:
    jid: "{{ job_result.ansible_job_id }}"
  register: job_status
  until: job_status.finished
  retries: 5
  delay: 10
20. Strategy
Defines how Ansible runs tasks across hosts.

Linear Strategy (default)
YAML

strategy: linear
Runs tasks on one host at a time, in order.

Free Strategy
YAML

strategy: free
Runs tasks on all hosts as soon as they’re ready.

Serial (batch execution)
YAML

serial: 3
Run 3 hosts at a time.

21. Error Handling
You can control task behavior on failure.

Example: Ignore error
YAML

- name: Try stopping a service
  service:
    name: fake-service
    state: stopped
  ignore_errors: yes
Fail only if custom condition is true
YAML

- name: Conditional failure
  command: some_command
  register: cmd_out
  failed_when: cmd_out.rc != 0
Stop all on error
YAML

any_errors_fatal: true
22. Ansible Vault
Encrypt secrets like API keys, passwords, and certificates.

Encrypt a file:
Bash

ansible-vault encrypt secrets.yml
Decrypt/view/edit:
Bash

ansible-vault decrypt secrets.yml
ansible-vault view secrets.yml
ansible-vault edit secrets.yml
Use in playbook:
Bash

ansible-playbook site.yml --ask-vault-pass
ansible-playbook site.yml --vault-password-file ~/.vault_pass.txt
23. Plugins
Plugins extend Ansible’s core functionality.

Types:
Callback: Format output (e.g., json, yaml, minimal).

Connection: SSH, WinRM, local.

Lookup: Read external data (e.g., CSV, files).

Filter: Custom Jinja2 filters.

Set custom plugin paths:
Bash

export ANSIBLE_FILTER_PLUGINS=/custom/filter_plugins
export ANSIBLE_STDOUT_CALLBACK=json
Lookup Example:
YAML

- name: Read a value from CSV
  debug:
    msg: "{{ lookup('csvfile', 'user1 file=users.csv delimiter=,') }}"
24. Tags
Tags allow you to run part of a playbook.

Tag a task:
YAML

- name: Install NGINX
  apt:
    name: nginx
    state: present
  tags:
    - web
    - nginx
Run only tagged tasks:
Bash

ansible-playbook web.yml --tags nginx
Skip tagged tasks:
Bash

ansible-playbook web.yml --skip-tags debug





















Ansible - Complete Guide

Ansible is an open-source automation tool that helps in configuration management, application deployment, task automation, and orchestration.

Key Features

Agentless: Uses SSH or WinRM for communication.

Simple Syntax: Uses YAML.

Idempotent: Will only make changes when necessary.

Powerful & Scalable: Suitable for small setups to large enterprise deployments.

1. Ansible Core Concepts

1.1 Automation Areas

Provisioning - Setting up servers or cloud resources (e.g., launching AWS EC2 instances).

Configuration Management - Ensuring servers are configured to a defined state (e.g., nginx installed, SSH configured).

Continuous Delivery - Automating builds and deployments via pipelines.

App Deployment - Rolling out applications and dependencies across environments.

Security Compliance - Enforcing baseline OS-level security (e.g., disable root SSH, set correct permissions).

Example: Provisioning a web server

- name: Provision NGINX on Ubuntu
  hosts: webservers
  become: true
  tasks:
    - name: Install NGINX
      apt:
        name: nginx
        state: present

2. Configuration Files

Default Config File: /etc/ansible/ansible.cfg

Precedence Order:

$ANSIBLE_CONFIG=/path/to/file – Environment variable

./ansible.cfg – Local directory

~/.ansible.cfg – User home directory

/etc/ansible/ansible.cfg – Default system-wide config

Useful Commands:

ansible-config list  # Lists all Ansible configuration options with details
ansible-config view  # Shows the config file Ansible is currently using
ansible-config dump  # Dumps the actual values used by Ansible, including overrides

Example: Set more parallel connections (forks)

[defaults]
forks = 10
host_key_checking = False
retry_files_enabled = False

Explanation:

forks determines how many hosts Ansible can connect to in parallel

host_key_checking disables SSH prompt on first connection

retry_files_enabled disables creation of .retry files

3. YAML (Yet Another Markup Language)

YAML is used to write playbooks, inventories, and variable files.

Syntax Example:

Fruit: Apple  # Key-Value
Vegetable: Carrot
liquid: Water
Meat: Chicken

List Example:

fruits:
  - Apple
  - Banana
  - Mango

Tip: Always use 2 spaces for indentation. No tabs.

4. Ansible Inventory

Inventory defines the systems managed by Ansible.

INI Format:

[webservers]
web1.example.com ansible_user=ubuntu
web2.example.com ansible_user=ubuntu

[dbservers]
db1.example.com ansible_user=root

YAML Format:

all:
  children:
    webservers:
      hosts:
        web1.example.com:
        web2.example.com:
    dbservers:
      hosts:
        db1.example.com:

SSH Login Example:

[webservers]
10.0.1.10 ansible_user=ec2-user ansible_ssh_private_key_file=~/.ssh/aws.pem

Command Example:

ansible webservers -m ping

-m ping: Use ping module to check connectivity

5. Ansible Variables (with Examples)

Variables make playbooks dynamic and reusable.

Defining Variables in Playbook

- name: Print a message
  hosts: localhost
  vars:
    greeting: "Hello from Ansible!"
  tasks:
    - name: Display variable
      debug:
        msg: "{{ greeting }}"

Defining Variables in Inventory

[webservers]
web1 ansible_host=192.168.1.100 app_port=8080

Defining in Group Vars

[webservers:vars]
app_name=myapp

Accessing in Playbook:

msg: "App name is {{ app_name }} and runs on port {{ app_port }}"

Boolean Example

vars:
  is_enabled: true

List and Dictionary

vars:
  packages:
    - nginx
    - git
  user:
    name: devops
    shell: /bin/bash

Using Variables in a Loop

- name: Install packages
  apt:
    name: "{{ item }}"
    state: present
  loop: "{{ packages }}"

6. Registering Variables and Variable Precedence

Register Output of a Task

You can capture the result of a task using register, and use it later.

Example:

- name: Get the uptime of the system
  command: uptime
  register: uptime_result

- name: Print result
  debug:
    var: uptime_result.stdout  # Displays the uptime output

Variable Precedence (from highest to lowest)

Extra vars (ansible-playbook site.yml -e "var=value")

Set facts

Include vars (via include_vars)

Role vars (defined within a role)

Play vars (inside the playbook vars: section)

Host facts (discovered via setup)

Host vars (defined in host_vars/ or inventory)

Group vars (defined in group_vars/ or inventory)

Role defaults (lowest priority)

Example:

ansible-playbook playbook.yml -e "env=production"

This overrides any env variable defined elsewhere.

Variable Defined in Inventory

[webservers:vars]
dns_server=10.5.5.3

[webservers]
web1 ansible_host=172.20.1.100 dns_server=10.5.5.5

In this case, web1 uses 10.5.5.5 because host-level variable overrides group-level.

7. Magic Variables and Scopes

Magic variables are built-in Ansible variables that give information about hosts, groups, and inventory structure.

Hostvars (Access data from other hosts)

- name: Print DNS of another host
  debug:
    msg: "DNS of web2: {{ hostvars['web2'].dns_server }}"

hostvars is a dictionary of host-specific variables.

Groups and Inventory

- name: Print all hosts in 'webservers'
  debug:
    var: groups['webservers']

- name: Print the groups of current host
  debug:
    var: group_names

- name: Print the name of the host
  debug:
    var: inventory_hostname

Facts from Another Host

- name: Print CPU info from web2
  debug:
    msg: "CPU: {{ hostvars['web2']['ansible_facts']['processor'][0] }}"

Scope Types:

Host Scope: Variables tied to a specific host

Play Scope: Variables available only during a play

8. Ansible Facts

Facts are system data (hostname, IP, OS, etc.) gathered by Ansible from managed nodes.

Enable Fact Gathering (default)

gather_facts: true

Disable Fact Gathering

gather_facts: false

Accessing Facts

- name: Show default IP
  debug:
    msg: "Default IPv4: {{ ansible_default_ipv4.address }}"

Gathering Modes (via config or ansible.cfg)

implicit: default, facts gathered unless turned off

explicit: gather only when you write gather_facts: true

smart: facts are reused unless missing

Command Line:

ansible localhost -m setup  # Gather and print all facts

9. Ansible Playbooks

Playbooks are YAML files that define sets of instructions for Ansible to follow. They allow you to orchestrate multiple tasks, control configuration, deploy applications, and much more.

Playbook Structure:

Play: Applies tasks to a group of hosts

Task: Executes a single module on the host

Module: The unit of work (like yum, apt, copy, etc.)

Handlers: Triggered only when notified by a task

Variables: Provide dynamic input to tasks

Example:

- name: Install and configure NGINX
  hosts: webservers
  become: true
  vars:
    nginx_port: 80
  tasks:
    - name: Install NGINX
      apt:
        name: nginx
        state: present

    - name: Ensure NGINX is running
      service:
        name: nginx
        state: started
        enabled: true

    - name: Open firewall port
      ufw:
        rule: allow
        port: "{{ nginx_port }}"
        proto: tcp

This playbook installs NGINX, starts it, enables it on boot, and opens the port.

10. Modules

Modules are the actual units of work in Ansible. Each task in a playbook invokes a module.

Commonly Used Modules:

System: user, group, service, hostname

Package: apt, yum, dnf, pip

File: copy, template, file, lineinfile

Command: command, shell, script, raw

Example: File Module

- name: Create a directory
  file:
    path: /opt/myapp
    state: directory
    owner: ubuntu
    mode: '0755'

Example: Copy Module

- name: Copy config file
  copy:
    src: nginx.conf
    dest: /etc/nginx/nginx.conf

Example: Template Module (with Jinja2)

- name: Deploy nginx config
  template:
    src: templates/nginx.conf.j2
    dest: /etc/nginx/nginx.conf

Templates allow variable substitution using Jinja2 syntax:

server {
    listen {{ nginx_port }};
    server_name {{ server_name }};
}

11. Playbook Execution Modes

These modes help you test your playbooks safely before applying changes.

Check Mode (Dry Run)

ansible-playbook site.yml --check

This simulates the execution and shows what changes would happen without making any actual changes.

Diff Mode (Show Before/After Changes)

ansible-playbook site.yml --diff

Useful for file-based tasks (e.g., template, lineinfile) to show differences.

Syntax Check

ansible-playbook site.yml --syntax-check

Validates the YAML structure of your playbook.

12. Loops & Conditionals

Simple Loop (List)

- name: Install packages
  apt:
    name: "{{ item }}"
    state: present
  loop:
    - git
    - curl
    - htop

Loop with Dictionary

- name: Create users
  user:
    name: "{{ item.name }}"
    uid: "{{ item.uid }}"
  loop:
    - { name: alice, uid: 1001 }
    - { name: bob, uid: 1002 }

Legacy Loop with with_items

- name: Install packages (legacy)
  apt:
    name: "{{ item }}"
    state: present
  with_items:
    - git
    - curl

Using Conditions

- name: Restart service if enabled
  service:
    name: apache2
    state: restarted
  when: is_enabled

Example: Only install if on Debian:

- name: Install Apache
  apt:
    name: apache2
    state: present
  when: ansible_os_family == 'Debian'

13. Handlers

Handlers are tasks that run only when notified. They’re typically used for restarting services after a configuration change.

Example:

- name: Install NGINX
  apt:
    name: nginx
    state: present
  notify: restart nginx

handlers:
  - name: restart nginx
    service:
      name: nginx
      state: restarted

When is a handler triggered?

Handlers are triggered only if the task using notify reports a change. If the task does not change the system state, the handler is skipped.

14. Roles

Roles are structured directories that allow easy reuse and sharing of automation content.

Role Directory Layout:

roles/
  apache/
    tasks/
      main.yml
    handlers/
      main.yml
    templates/
      httpd.conf.j2
    vars/
      main.yml
    defaults/
      main.yml

Create a Role:

ansible-galaxy init apache

Use a Role in Playbook:

- name: Deploy web server
  hosts: webservers
  roles:
    - apache

Install a Public Role:

ansible-galaxy install geerlingguy.apache

15. Collections

Collections are packages that bundle modules, roles, and plugins together.

Install a Collection:

ansible-galaxy collection install community.general

Use Modules from a Collection:

- name: Use sysctl from community.general
  community.general.sysctl:
    name: net.ipv4.ip_forward
    value: '1'
    state: present

16. Templating with Jinja2

Templating in Ansible is done using the Jinja2 engine. You can embed variables and expressions inside templates, which are typically used with the template module.

Template File Example (nginx.conf.j2):

server {
    listen {{ nginx_port }};
    server_name {{ server_name }};
    root {{ document_root }};
}

Template Task in Playbook:

- name: Deploy NGINX config
  template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf

Common Filters in Jinja2:

{{ user_name | upper }}     # Converts to uppercase
{{ list_of_users | join(', ') }}  # Joins list with comma
{{ 3.14159 | round(2) }}    # Rounds to 2 decimal places

17. Environment Setup (Local Dev/Test)

You can simulate Ansible environments using tools like:

1. Vagrant

Used to create reproducible local environments.

vagrant init ubuntu/bionic64
vagrant up

2. Docker

Run containers for testing roles or modules.

docker run -it ubuntu:latest bash

3. VMware/VirtualBox

Useful for simulating real infrastructure with virtual machines.

18. Variable File Separation

You can store variables in separate files for better organization.

Using vars_files:

- name: Example with vars file
  hosts: localhost
  vars_files:
    - vars/common.yml
  tasks:
    - name: Print custom message
      debug:
        msg: "{{ welcome_message }}"

Using host_vars and group_vars:

Create directory named host_vars/ and group_vars/

Add YAML file with hostname or group name

Example: host_vars/web1.yml

nginx_port: 80
server_name: web1.example.com

19. Async Actions

Some tasks take a long time to run (e.g., backups, updates). You can run them asynchronously without blocking.

Example:

- name: Run a long shell script asynchronously
  command: /opt/long_script.sh
  async: 600   # Max seconds to run
  poll: 0      # Fire and forget
  register: job_result

- name: Check async job status
  async_status:
    jid: "{{ job_result.ansible_job_id }}"
    register: job_status
    until: job_status.finished
    retries: 5
    delay: 10

20. Strategy

Defines how Ansible runs tasks across hosts.

Linear Strategy (default)

strategy: linear

Runs tasks on one host at a time, in order.

Free Strategy

strategy: free

Runs tasks on all hosts as soon as they’re ready.

Serial (batch execution)

serial: 3

Run 3 hosts at a time.

21. Error Handling

You can control task behavior on failure.

Example: Ignore error

- name: Try stopping a service
  service:
    name: fake-service
    state: stopped
  ignore_errors: yes

Fail only if custom condition is true

- name: Conditional failure
  command: some_command
  register: cmd_out
  failed_when: cmd_out.rc != 0

Stop all on error

any_errors_fatal: true

22. Ansible Vault

Encrypt secrets like API keys, passwords, and certificates.

Encrypt a file:

ansible-vault encrypt secrets.yml

Decrypt/view/edit:

ansible-vault decrypt secrets.yml
ansible-vault view secrets.yml
ansible-vault edit secrets.yml

Use in playbook:

ansible-playbook site.yml --ask-vault-pass
ansible-playbook site.yml --vault-password-file ~/.vault_pass.txt

23. Plugins

Plugins extend Ansible’s core functionality.

Types:

Callback: Format output (e.g., json, yaml, minimal)

Connection: SSH, WinRM, local

Lookup: Read external data (e.g., CSV, files)

Filter: Custom Jinja2 filters

Set custom plugin paths:

export ANSIBLE_FILTER_PLUGINS=/custom/filter_plugins
export ANSIBLE_STDOUT_CALLBACK=json

Lookup Example:

- name: Read a value from CSV
  debug:
    msg: "{{ lookup('csvfile', 'user1 file=users.csv delimiter=,') }}"

24. Tags

Tags allow you to run part of a playbook.

Tag a task:

- name: Install NGINX
  apt:
    name: nginx
    state: present
  tags:
    - web
    - nginx

Run only tagged tasks:

ansible-playbook web.yml --tags nginx

Skip tagged tasks:
ansible-playbook web.yml --skip-tags debug

Do not change anything, please provide me readme.md 
