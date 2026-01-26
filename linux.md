# Linux

***Operating System (OS)***
- An operating system (OS) is system software that acts as an intermediary between computer hardware and the user. It provides a user interface and manages the hardware resources so that applications can run smoothly and efficiently.

***Quick Explanation***
- Acts as a bridge: It connects the user and the physical hardware.
- Provides a user interface: Such as GUI (Windows, macOS) or CLI (Linux terminal).
- Manages hardware: CPU, memory, storage, I/O devices.
- Runs software: Ensures applications get the resources they need without interfering with each other.

***Linux***
- Linux is a free, open-source operating system similar to Windows or macOS but more flexible. It is known for its stability, security, and ability to be modified by anyone. Because it’s open-source, many different versions called “distributions” exist, created and improved by a large community of developers and users.

***Linux vs. Windows***
| Feature          | **Linux**                               | **Windows**                                   |
| ---------------- | --------------------------------------- | --------------------------------------------- |
| **Price**        | Free                                    | Paid ($$$)                                    |
| **Ease of Use**  | Less user-friendly for beginners        | Very user-friendly                            |
| **Reliability**  | Very reliable; can run for months/years | Often requires reboots                        |
| **Software**     | Mostly enterprise and developer tools   | Large variety: Office, games, utilities, etc. |
| **Multitasking** | Excellent multitasking                  | Good, but uses more CPU/RAM                   |
| **Security**     | Very secure                             | Moderately secure                             |
| **Open Source**  | Yes, fully open-source                  | No, closed-source                             |

***Linux Structure***
***1. Hardware (Innermost Layer)***
- Linux follows a layered architecture, where each layer has a specific role. This design makes Linux portable, secure, efficient, and modular.
  - This is the physical machine:
    - CPU, Memory, Storage, Network devices, I/O devices
  - It provides the raw computational resources needed to run the operating system.

***2. Kernel (Core of Linux)***
- The kernel is the heart of the Linux OS. It sits between hardware and user applications. It handles:
***Hardware Abstraction**
- Programs don’t access hardware directly. Instead, they request services from the kernel.
- Key Responsibilities of the Kernel
  - Process management (create, schedule, kill processes)
  - Memory management (allocate, protect memory regions)
  - File system management (open, read, write, close files)
  - Device management (interact with drivers, provide device access)
  - Resource allocation (CPU time, memory limits, quotas, I/O control)
  - Security and permissions
- Because the kernel abstracts hardware, well-written programs can run on different hardware without change (portability).

***3. System Calls (API between User Space & Kernel Space)***
- User programs can't interact directly with hardware. Instead, they use system calls, which act as a gateway to the kernel.
- Examples of System Calls
  - File operations
    - open(), close()
    - read(), write()
    - link()
    - exec()
  - Process management
    - fork() – create a new process
    - kill() – terminate a process
    - wait() – wait for a process to finish
  - Directory & permission management
    - chdir() – change directory
    - chmod() – change permissions
    - chown() – change file owner
  - System resource operations
    - Setting memory limits
    - Updating accounting logs
    - Accessing device files
- System calls ensure controlled, secure, and standardized access to hardware resources.

***4. User Space (User Programs, Shells, Applications)***
- This is where all your regular applications run:
  - Bash shell
  - Editors (vim, nano)
  - Tools (curl, grep, tar)
  - Services (nginx, docker)
  - Developer applications
- These applications run in user mode, which has limited privileges, ensuring system safety.
- User programs rely on system calls to perform privileged operations via the kernel.

***Summary***
- Linux is a layered, modular, multi-user, multi-tasking operating system where:
  - Hardware is the foundation.
  - Kernel interacts with hardware and provides services.
  - System calls act as the interface between user applications and the kernel.
  - User programs run in isolation, relying on the kernel for resources.
- This architecture makes Linux secure, portable, stable, and widely used in servers, cloud systems, embedded devices, and more.

***Linux File System – Simplified & Structured Explanation***
***1. What is a File System?***
- A file system is a logical method of organizing and storing files on a disk or partition.
- It controls how data is stored, named, accessed, and managed.
- It resides inside a partition, which is a logical section of a physical disk.
- A single disk can have multiple partitions, and each partition usually has one file system (e.g., /, /home, /var).
- Examples of Linux File Systems
  - ext4 (most common), xfs, btrfs, vfat, ntfs (Windows type, Linux can mount it)

***2. Partitions and File Systems***
- A partition is like a container where one file system is stored.
- For example:
  - One partition might hold the root file system /
  - Another might store /home
  - Another might store /var
***Why separate partitions?***
- Better security (user files separate from system files)
- Prevent one directory from filling the whole disk
- Easier backups and recovery
- Can use different file systems for different directories

***3. Everything is a File in Linux***
- Linux treats everything as a file, including:
  - Normal files, Directories, Devices (DVD, USB, keyboard, disk), Sockets, pipes, links
- This is why /dev contains files like sda, sdb, tty, which are actually device interfaces.
- Linux follows a hierarchical file system, similar to an upside-down tree:
  ```arduino
    /
    ├── bin
    ├── etc
    ├── home
    ├── var
    └── usr
  ```
***5. What is an Inode?***
- An inode is a data structure that stores metadata about a file, such as:
  - File type, Ownership (UID, GID), Permissions, File size, Timestamps, Disk block pointers
- Inodes do NOT store: File names, File data
- The directory stores file names and maps them to inode numbers.
- Unique Identification
  - Each file is uniquely identified by:
    - Its inode number
    - The directory where the name resides
- Thus, different directories can have files with the same name, but their inode numbers differ.
- By Linux convention:
  - Inode 2 → root directory (/)
  - Inode 3 → lost+found
  - Inode 0 and 1 → unused
- View Inode Numbers:
  ```bash
  ls -i
  ```
***7. Self-Contained Filesystems***
- Each filesystem is independent and self-contained:
- No dependencies exist between one filesystem and another.
- You can mount, unmount, backup, or repair a filesystem independently.
  - Example:
    - /home on one partition
    - /var on another
- If /home gets full or corrupted, it doesn’t affect /var.

****Summary***
- A file system organizes files on a partition.
- Linux uses multiple file systems, each on its own partition.
- Linux directory structure is hierarchical, with root / at the top.
- Everything is a file, including devices.
- Files are identified by inode numbers, not just filenames.
- Each file system is independent and contains its own root directory and inodes.

***Linux File Naming Rules***
- Allowed characters: Most characters are permitted, but you should avoid spaces, tabs, and special shell characters such as:
  ```& ; ( ) | ? \ ' " \ [ ] { } < > $ - ! /```
- Case sensitive: File names like NOVEMBER, November, and november are all different.
- Length limit: File names can be up to 256 characters long.
- Extensions: Extensions are optional but commonly used to indicate file type. Examples:
  - libc.a → archive/library
  - program.c → C source file
  - alpha2.f → Fortran source
  - xwd2ps.o → object file
  - mygames.Z → compressed file
- Hidden files: Any file beginning with a dot (.) is hidden, e.g., .cshrc, .login.
- Uniqueness:
  - Two files in the same directory cannot have the same name.
  - Files in different directories may share names.
- Reserved filenames:
  - / → root directory
  - . → current directory
  - .. → parent directory
  - ~ → user’s home directory

***What is Root in Linux?***
- Root is the administrator or superuser with unrestricted access to all commands, files, directories, and resources.

***Changing Passwords in Linux***
- To change another user’s password, you must be logged in as root (or use sudo).
  ```bash
  passwd username
  ```
- Any user can change their own password using:
  ```bash
  passwd
  ```
***Absolute vs Relative Path***
- Absolute path = Full path from root → Always starts with /
- Relative path = Based on current directory → Does NOT start with /
- Use absolute for scripts, relative for quick navigation

***Linux File System Navigation + File & Directory Commands***

📌 Navigation, File Handling, and Directory Management Command

| Command         | Purpose / Meaning              | Example                | Output / Explanation             |
| --------------- | ------------------------------ | ---------------------- | -------------------------------- |
| **pwd**         | Show current working directory | `pwd`                  | `/home/user/projects`            |
| **ls**          | List files and directories     | `ls`                   | Shows items in current folder    |
| **ls -l**       | Detailed list with permissions | `ls -l`                | Permissions, owner, size, date   |
| **ls -a**       | Show hidden files              | `ls -a`                | Shows `.bashrc`, `.gitconfig`    |
| **cd dir**      | Go into directory              | `cd Documents`         | Enters Documents folder          |
| **cd ..**       | Move one level up              | `cd ..`                | `/home/user/docs` → `/home/user` |
| **cd /**        | Go to root directory           | `cd /`                 | Moves to system root             |
| **cd ~**        | Go to home directory           | `cd ~`                 | Moves to `/home/user`            |
| **cd -**        | Go to previous directory       | `cd -`                 | Like a "back" button             |
| **tree**        | Show directory tree            | `tree`                 | Displays folder structure        |
| **find**        | Search files/folders           | `find . -name "*.txt"` | Finds all `.txt` files           |
| **locate file** | Fast search using DB           | `locate passwd`        | Lists paths containing "passwd"  |
| **du -sh**      | Folder size (human)            | `du -sh`               | e.g., `2.3G .`                   |
| **df -h**       | Disk usage                     | `df -h`                | Disk space in human format       |
| **stat file**   | File metadata                  | `stat file.txt`        | Shows inode, timestamps          |
| **file file**   | Detect file type               | `file script.sh`       | e.g., “Bourne shell script”      |

✅ 📁 DIRECTORY CREATION & MANAGEMENT COMMANDS
| Command           | Purpose                      | Example          | Explanation                      |
| ----------------- | ---------------------------- | ---------------- | -------------------------------- |
| **mkdir dir**     | Create a directory           | `mkdir photos`   | Creates folder "photos"          |
| **mkdir -p path** | Create nested dirs           | `mkdir -p a/b/c` | Creates a → b → c                |
| **rmdir dir**     | Remove *empty* directory     | `rmdir test`     | Only works if directory is empty |
| **rm -r dir**     | Remove directory with files  | `rm -r project`  | Deletes directory recursively    |
| **rm -rf dir**    | Force delete dir (dangerous) | `rm -rf system`  | Deletes without asking           |

✅ 📄 FILE CREATION COMMANDS
| Command                 | Purpose                               | Example                        | Explanation              |
| ----------------------- | ------------------------------------- | ------------------------------ | ------------------------ |
| **touch file**          | Create empty file or update timestamp | `touch notes.txt`              | Creates `notes.txt`      |
| **echo "text" > file**  | Create/overwrite file                 | `echo "Hello" > hello.txt`     | Writes "Hello" into file |
| **echo "text" >> file** | Append text                           | `echo "Add this" >> hello.txt` | Adds text to file        |
| **nano file**           | Create/Edit file in nano editor       | `nano test.sh`                 | Opens nano               |
| **vi file**             | Create/Edit file using vi             | `vi config.txt`                | Opens vi/vim             |

✅ 📂 FILE COPYING, MOVING, AND DELETING

| Command                 | Purpose                      | Example                  | Explanation                   |
| ----------------------- | ---------------------------- | ------------------------ | ----------------------------- |
| **cp src dest**         | Copy file                    | `cp file.txt backup.txt` | Copies file.txt to backup.txt |
| **cp file1 file2 dir/** | Copy files to folder         | `cp a.txt b.txt docs/`   | Copies both to docs           |
| **cp -r dir dest**      | Copy folder recursively      | `cp -r project/ backup/` | Copies entire folder          |
| **mv src dest**         | Move/rename file or folder   | `mv a.txt b.txt`         | Renames a.txt → b.txt         |
| **mv file dir/**        | Move file to directory       | `mv file.txt /tmp/`      | Moves file                    |
| **rm file**             | Remove file                  | `rm data.txt`            | Deletes file                  |
| **rm -f file**          | Force delete without warning | `rm -f important.txt`    | Deletes file immediately      |

***Wildcards in Linux (Globbing)***
- Wildcards are special characters used to match one or more filenames in shell commands.

| Wildcard  | Meaning                                       | Example             | Matches                               |
| --------- | --------------------------------------------- | ------------------- | ------------------------------------- |
| `*`       | Zero or more characters                       | `ls *.txt`          | file.txt, notes.txt, test123.txt      |
| `?`       | Exactly one character                         | `ls file?.txt`      | file1.txt, fileA.txt (not file10.txt) |
| `[]`      | Match any one character from the set or range | `ls file[1-3].txt`  | file1.txt, file2.txt, file3.txt       |
| `{}`      | Brace expansion (not a wildcard, but useful)  | `echo {1..5}`       | 1 2 3 4 5                             |
| `{a,b,c}` | Multiple options                              | `cp file.{txt,log}` | expands to file.txt and file.log      |

***Linux File Types***
- You can see file types using:
  ```bash
  ls -l
  ```
- Example of ls-l:
  ```lua
  -rw-r--r--  file.txt       → regular file
  drwxr-xr-x  myfolder       → directory
  lrwxrwxrwx  link -> file   → soft link
  crw-------  tty0           → character device
  brw-rw----  sda            → block device
  srwxr-xr-x  docker.sock    → socket
  prw-r--r--  myfifo         → named pipe
  ```
| Symbol | Type                      | Meaning                                       | Example                               |
| ------ | ------------------------- | --------------------------------------------- | ------------------------------------- |
| **-**  | Regular file              | Text, binary, scripts, etc.                   | `document.txt`, `script.sh`           |
| **d**  | Directory                 | Folder containing files/directories           | `/home`, `/etc`                       |
| **l**  | Symbolic link (soft link) | Points to another file or directory           | `myfile → /etc/passwd`                |
| **c**  | Character device file     | Hardware device transferring 1 byte at a time | Keyboard, mouse `/dev/tty`            |
| **b**  | Block device file         | Hardware handling data in blocks              | Hard disks `/dev/sda`, USB `/dev/sdb` |
| **s**  | Socket                    | Network communication between processes       | `/var/run/docker.sock`                |
| **p**  | Named pipe (FIFO)         | Inter-process communication inside system     | `/tmp/mypipe`                         |

***Hard Link vs Soft Link (Symbolic Link)***

  <img width="829" height="592" alt="image" src="https://github.com/user-attachments/assets/562dc080-5eda-4e26-8ce2-493d61ee4ba4" />

***Command Syntax***
	⁃	Cmd options arguments

***File Permissions***

| Command   | Purpose                 | Example                     | Meaning                                            |
| --------- | ----------------------- | --------------------------- | -------------------------------------------------- |
| **chmod** | Change file permissions | `chmod 644 file`            | Owner = **rw-**, Group = **r--**, Others = **r--** |
|           |                         | `chmod u+rwx,g+rx,o+r file` | u = **rwx**, g = **r-x**, o = **r--**              |
|           |                         | `chmod o-rwx file`          | Remove all permissions from **others**             |
| **chown** | Change file owner/group | `chown user:grp file`       | Owner = **user**, Group = **grp**                  |
|           |                         | `chown satish file`         | Change only owner                                  |
| **chgrp** | Change group only       | `chgrp grp file`            | File now belongs to group **grp**                  |
|           |                         | `chgrp developers file`     | Only group changes, owner stays same               |

***ACL***
- ACLs (Access Control Lists) allow you to assign fine-grained, flexible permissions to files and directories — beyond the traditional owner / group / others permission model.

🔥 Why ACL is Used (Main Purposes)
1. Give permissions to specific users without changing file ownership
- Normally, permissions apply only to:
  - Owner
  - Group
  - Others
- ACL allows:
  - Add permissions for extra users
  - Add permissions for extra groups
- Example:
  - File belongs to user ubuntu and group dev,
  - but you want to give john read+write:
  ```bash
  setfacl -m u:john:rw file.txt
  setfacl -m g:design:r file.txt
  setfacl -m d:g:developers:rwx /project //Set default permissions for new files in a directory
  setfacl -m u:alice:r /secure/report.pdf //Provide controlled access without giving full ownership
  ```

| Task            | Command                         | Example                   |          |
| --------------- | ------------------------------- | ------------------------- | -------- |
| View ACL        | `getfacl file`                  | `getfacl doc.txt`         |          |
| Add user ACL    | `setfacl -m u:user:perm file`   | `u:john:rw`               |          |
| Add group ACL   | `setfacl -m g:group:perm file`  | `g:dev:rwx`               |          |
| Remove user ACL | `setfacl -x u:user file`        | `u:john`                  |          |
| Remove all ACL  | `setfacl -b file`               | `file.txt`                |          |
| Default ACL     | `setfacl -m d:u:user:perm dir`  | `d:u:john:rwx`            |          |
| Recursive ACL   | `setfacl -R -m u:user:perm dir` | `u:john:rw`               |          |
| Copy ACL        | `getfacl src                    | setfacl --set-file=- dst` | copy ACL |

## 🔐 Special Permissions in Linux

| Bit            | Name         | Value |
| -------------- | ------------ | ----- |
| **SUID**       | Set User ID  | **4** |
| **SGID**       | Set Group ID | **2** |
| **Sticky Bit** | Sticky       | **1** |


🧍‍♂️ SUID (Set User ID)

| Item                 | Description                          |
| -------------------- | ------------------------------------ |
| Applies to           | **Executable files only**            |
| Purpose              | Program runs as **file owner**       |
| Execute bit required | ✅ Yes                               |
| Symbolic command     | `chmod u+s file`                     |
| Numeric format       | `chmod 4xxx file`                    |
| Example              | `chmod 4755 file`                    |
| ls output            | `-rwsr-xr-x`                         |
| Real example         | `/usr/bin/passwd`                    |
| Security risk        | High (misuse → privilege escalation) |

🧍‍♂️  SGID on Files

| Item                 | Description                    |
| -------------------- | ------------------------------ |
| Applies to           | Executable files               |
| Purpose              | Program runs as **file group** |
| Execute bit required | ✅ Yes                          |
| Symbolic command     | `chmod g+s file`               |
| Numeric format       | `chmod 2xxx file`              |
| Example              | `chmod 2755 file`              |
| ls output            | `-rwxr-sr-x`                   |


| Item                 | Description                           |
| -------------------- | ------------------------------------- |
| Applies to           | **Directories**                       |
| Purpose              | New files inherit **directory group** |
| Execute bit required | ❌ No                                  |
| Symbolic command     | `chmod g+s dir`                       |
| Numeric format       | `chmod 2775 dir`                      |
| ls output            | `drwxrwsr-x`                          |
| Common use           | Shared team directories               |

🧹 Sticky Bit

| Item                 | Description                           |
| -------------------- | ------------------------------------- |
| Applies to           | **Directories only**                  |
| Purpose              | Prevents users deleting others’ files |
| Who can delete files | File owner, dir owner, root           |
| Execute bit required | ❌ No                                  |
| Symbolic command     | `chmod +t dir`                        |
| Numeric format       | `chmod 1xxx dir`                      |
| Example              | `chmod 1777 /tmp`                     |
| ls output            | `drwxrwxrwt`                          |

## Archive vs Compress vs Backup

| Term               | What It Means                    | Tool Used      | Example Command               | Result         |
| ------------------ | -------------------------------- | -------------- | ----------------------------- | -------------- |
| Archive            | Combine many files into one file | `tar`          | `tar -cvf data.tar dir`       | `data.tar`     |
| Compress           | Reduce file size                 | `gzip`         | `gzip file.txt`               | `file.txt.gz`  |
| Archive + Compress | Combine + reduce size            | `tar + gzip`   | `tar -czvf data.tar.gz dir`   | `data.tar.gz`  |
| Backup             | Copy data for recovery           | `tar`, `rsync` | `tar -czvf backup.tar.gz dir` | Backup created |
| Unpack             | Extract archived files           | `tar`          | `tar -xvf data.tar`           | Files restored |
| Uncompress         | Restore compressed file          | `gunzip`       | `gunzip file.gz`              | Original file  |

| Option | Meaning | Explanation                    |
| ------ | ------- | ------------------------------ |
| `c`    | Create  | Creates a new archive          |
| `x`    | Extract | Extracts archive contents      |
| `z`    | gzip    | Compress/uncompress using gzip |
| `v`    | Verbose | Shows files being processed    |
| `f`    | File    | Archive filename is specified  |


## Remote File Copy & Backup

| Scenario                   | Command                                                                    | Tool         | What It Does                        | When to Use                 | Result on Remote Server         |
| -------------------------- | -------------------------------------------------------------------------- | ------------ | ----------------------------------- | --------------------------- | ------------------------------- |
| Copy a single file         | `scp file.txt user@remote_ip:/backup/`                                     | scp          | Copies one file securely over SSH   | One-time small file copy    | `file.txt` copied to `/backup/` |
| Copy a directory           | `scp -r /opt/app user@remote_ip:/backup/`                                  | scp          | Recursively copies entire directory | Small directories, rare use | `/backup/app/` created          |
| Incremental backup         | `rsync -av /data/ user@remote_ip:/backup/data/`                            | rsync        | Copies only new/changed files       | Daily backups               | `/backup/data/` updated         |
| Backup with compression    | `rsync -avz /data/ user@remote_ip:/backup/data/`                           | rsync        | Compresses data during transfer     | Slow networks / WAN         | Faster transfer, same data      |
| Backup with date           | `rsync -av /opt/app user@remote_ip:/backup/app_$(date +%F)/`               | rsync        | Creates dated backup folder         | Snapshot-style backups      | `/backup/app_YYYY-MM-DD/`       |
| Streamed compressed backup | `tar -czf - /opt/app \| ssh user@remote_ip "cat > app_$(date +%F).tar.gz"` | tar + ssh    | Compresses & sends data directly    | Large data, no temp files   | `.tar.gz` backup file           |
| Automated daily backup     | `0 2 * * * rsync -avz /data user@remote_ip:/backup/data`                   | cron + rsync | Runs backup every day at 2 AM       | Production automation       | Daily updated backup            |


## 👤👥 User & Group Management

| Topic                  | What It Is                      | Why We Need It (Real World)                     | Command / Example            | Result                           |
| ---------------------- | ------------------------------- | ----------------------------------------------- | ---------------------------- | -------------------------------- |
| User                   | Individual system identity      | Separate people/apps, security & accountability | `useradd -m devuser`         | User `devuser` created with home |
| Password               | User authentication             | Prevent unauthorized access                     | `passwd devuser`             | Password set                     |
| Verify user            | Check UID, GID, groups          | Troubleshooting & audits                        | `id devuser`                 | Shows user details               |
| Group                  | Collection of users             | Shared access management                        | `groupadd devops`            | Group `devops` created           |
| Add user to group      | Assign group permissions        | Team-based access                               | `usermod -aG devops devuser` | User added to group              |
| Group verification     | Check memberships               | Confirm access rights                           | `groups devuser`             | Lists groups                     |
| System users           | Service accounts                | Run apps securely (non-root)                    | `mysql`, `nginx` users       | App isolation                    |
| Shared directory       | Group-collaboration space       | Team file sharing                               | `mkdir /opt/app`             | Shared dir created               |
| Change group ownership | Assign directory to group       | Control access                                  | `chown :devops /opt/app`     | Group ownership set              |
| SGID on directory      | Enforce group inheritance       | Team consistency                                | `chmod 2775 /opt/app`        | Files inherit group              |
| Lock user              | Disable login                   | Security incident response                      | `usermod -L devuser`         | User locked                      |
| Unlock user            | Restore access                  | Post-incident recovery                          | `usermod -U devuser`         | User unlocked                    |
| Delete user            | Remove access                   | Offboarding                                     | `userdel -r devuser`         | User & home removed              |
| Root user              | Superuser (UID 0)               | System administration                           | `sudo su -`                  | Full privileges                  |
| sudo                   | Controlled privilege escalation | Safer than root login                           | `sudo yum install nginx`     | Admin task allowed               |
| Auditing               | Track responsibility            | Compliance & security                           | User-based ownership         | Accountability ensured           |
| Best practice          | Least privilege                 | Prevent system compromise                       | One user per app             | Secure system                    |

## ⚙️ Linux Process Management

| Topic              | What It Means                       | Why It Matters (Real World)          | Command / Example                | Result / Output       |                |
| ------------------ | ----------------------------------- | ------------------------------------ | -------------------------------- | --------------------- | -------------- |
| Process            | Running instance of a program       | OS executes tasks via processes      | `nginx` running                  | Process in memory     |                |
| Program vs Process | Program = file, Process = execution | Same program can have many processes | `/usr/bin/nginx` → PID 1234      | Active PID            |                |
| PID                | Unique process ID                   | Identify, monitor, kill processes    | `ps -ef`                         | PID shown             |                |
| PPID               | Parent process ID                   | Shows hierarchy                      | `ps -o pid,ppid,cmd`             | Parent-child view     |                |
| Process creation   | Fork + exec by kernel               | How commands start                   | `ls`                             | Child process created |                |
| View all processes | List running processes              | Auditing & troubleshooting           | `ps -ef`                         | Full process list     |                |
| Live monitoring    | Dynamic CPU/memory view             | Detect high load                     | `top` / `htop`                   | Live stats            |                |
| Find process       | Search by name                      | Check if service running             | `ps -ef \| grep nginx`           | Matching PID          |                |
| Daemon process     | Background service                  | Servers & system services            | `nginx`, `sshd`                  | Always running        |                |
| User process       | Started by user                     | Interactive work                     | `vi`, `ls`                       | Tied to terminal      |                |
| System process     | Core OS tasks                       | System stability                     | `systemd`, `udevd`               | OS functions          |                |
| Kernel process     | Kernel threads                      | Hardware & scheduling                | `kthreadd`                       | Kernel managed        |                |
| Child process      | Spawned by parent                   | Scalability                          | nginx worker                     | Load handling         |                |
| Orphan process     | Parent exited                       | Needs cleanup                        | PPID = 1                         | Adopted by systemd    |                |
| Zombie process     | Finished, not reaped                | Resource leak                        | State = Z                        | Needs parent restart  |                |
| Identify orphans   | PPID = 1                            | Debug lingering processes            | `ps -ef                          | awk '$3==1'`          | Orphans listed |
| Remove orphan      | Kill or restart parent              | Clean system                         | `kill PID` / restart service     | Process removed       |                |
| Remove zombie      | Restart parent                      | Kernel cleanup                       | `systemctl restart service`      | Zombie cleared        |                |
| Graceful stop      | Ask process to exit                 | Prevent data loss                    | `kill PID`                       | SIGTERM sent          |                |
| Force stop         | Immediate kill                      | Hung processes                       | `kill -9 PID`                    | Process killed        |                |
| Kill by name       | Kill all matching                   | Quick service stop                   | `pkill nginx`                    | All nginx stopped     |                |
| Signals            | Process control messages            | Reload, stop, pause                  | `kill -HUP PID`                  | Config reloaded       |                |
| SIGTERM            | Signal 15                           | Safe shutdown                        | `kill PID`                       | Graceful exit         |                |
| SIGKILL            | Signal 9                            | Emergency stop                       | `kill -9 PID`                    | Forced exit           |                |
| SIGHUP             | Signal 1                            | Reload config                        | `kill -HUP nginx`                | Zero downtime         |                |
| Foreground job     | Runs in terminal                    | Interactive commands                 | `sleep 100`                      | Terminal blocked      |                |
| Background job     | Runs in background                  | Long tasks                           | `sleep 100 &`                    | Terminal free         |                |
| Job control        | Manage background jobs              | Resume tasks                         | `jobs`, `fg`                     | Job managed           |                |
| Nice value         | CPU priority                        | Protect production apps              | `nice -n 10 command`             | Lower priority        |                |
| Renice             | Change priority                     | Tune running app                     | `renice 10 -p PID`               | Priority updated      |                |
| Process ownership  | User running process                | Security isolation                   | `ps -ef                          | grep nginx`           | Owner shown    |
| Root vs non-root   | Privilege separation                | Reduce attack impact                 | nginx workers as `nginx`         | Safer system          |                |
| Service management | Managed daemons                     | Clean lifecycle                      | `systemctl restart nginx`        | Service restarted     |                |
| Real-world example | Web server                          | High availability                    | nginx master + workers           | Scalable model        |                |
| Real-world example | Database                            | Data safety                          | `systemctl stop mysql`           | Clean shutdown        |                |
| Real-world example | Backup job                          | Non-blocking                         | `tar -czf backup.tar.gz /data &` | Background job        |                |
| Common mistake     | Always kill -9                      | Data corruption                      | Bad practice                     | Risky system          |                |
| Best practice      | Graceful first                      | Stability                            | SIGTERM → SIGKILL                | Safe ops              |                |
| Interview key      | What is orphan?                     | Troubleshooting                      | Parent died                      | PPID = 1              |                |
| Interview key      | Zombie fix                          | OS hygiene                           | Restart parent                   | Zombie removed        |                |


## Linux Resource Management

| Resource           | What It Controls      | Common Production Symptoms       | What to Check (Order)         | Key Commands                      | What the Command Tells You | Real Production Example  | Fix / Remediation               |                         |
| ------------------ | --------------------- | -------------------------------- | ----------------------------- | --------------------------------- | -------------------------- | ------------------------ | ------------------------------- | ----------------------- |
| CPU                | Processing power      | App slow, high latency, timeouts | Load → CPU usage → process    | `uptime`                          | Load average               | Load 12 on 4-core server | Reduce load, scale CPU          |                         |
| CPU                | Processing power      | High CPU alerts                  | Per-process CPU               | `top`, `htop`                     | Which PID consuming CPU    | Java using 300% CPU      | Restart app, optimize code      |                         |
| CPU                | Processing power      | Uneven CPU usage                 | Per-core usage                | `mpstat -P ALL`                   | Hot CPU cores              | One core 100%            | Fix threading, scale            |                         |
| CPU                | Processing power      | CPU starvation                   | Scheduling priority           | `ps -eo pid,ni,cmd`               | Nice values                | Backup job starving app  | `renice 10 -p PID`              |                         |
| CPU                | Processing power      | Background jobs slow prod        | Priority control              | `nice -n 15 command`              | Lower priority             | Nightly backup           | Run with nice                   |                         |
| Memory             | RAM usage             | App crash, slow system           | Used/free memory              | `free -h`                         | RAM & swap usage           | RAM 98%, swap full       | Restart app, add RAM            |                         |
| Memory             | RAM usage             | Gradual slowdown                 | Per-process memory            | `top`, `htop`                     | Memory hog process         | Java using 60% RAM       | Tune heap, restart              |                         |
| Memory             | RAM usage             | Memory pressure                  | Paging activity               | `vmstat 1`                        | Swap in/out                | Constant swapping        | Add RAM                         |                         |
| Memory             | RAM usage             | Suspected leak                   | Sorted memory                 | `ps aux --sort=-%mem`             | Top memory users           | App leak                 | Fix leak                        |                         |
| OOM                | Memory protection     | App killed suddenly              | Kernel logs                   | `dmesg                            | grep -i oom`               | OOM killer evidence      | App killed overnight            | Increase RAM, limits    |
| Disk Space         | Storage               | App write failure                | Filesystem usage              | `df -h`                           | Disk usage %               | `/var` 100% full         | Cleanup logs                    |                         |
| Disk Space         | Storage               | Unknown usage                    | Directory size                | `du -sh /var/*`                   | Largest dirs               | Logs consuming space     | Archive/delete                  |                         |
| Disk Space         | Storage               | Hidden space usage               | Open deleted files            | `lsof                             | grep deleted`              | Files still open         | Log deleted but space not freed | Restart process         |
| Disk I/O           | Read/write speed      | High latency                     | I/O wait                      | `iostat -xz 1`                    | Disk wait time             | DB slow due to disk      | Move to SSD                     |                         |
| Disk I/O           | Read/write speed      | I/O hog                          | Per-process I/O               | `iotop`                           | Disk heavy PID             | Backup job               | Reschedule                      |                         |
| Files              | File usage            | Too many files                   | File count                    | `find /path -type f               | wc -l`                     | File explosion           | Millions of temp files          | Cleanup                 |
| Inodes             | File metadata         | Disk free but cannot write       | Inode usage                   | `df -i`                           | Inodes %                   | Inodes 100%              | Delete small files              |                         |
| Inodes             | File metadata         | App errors                       | Small files                   | `find /tmp -type f`               | Temp files                 | App creating temp files  | Fix app                         |                         |
| Network            | Connectivity          | App unreachable                  | Network reachability          | `ping host`                       | Packet loss/latency        | Packet loss seen         | Network fix                     |                         |
| Network            | Connectivity          | Port not accessible              | Listening ports               | `ss -tulnp`                       | Port & PID                 | App not listening        | Restart service                 |                         |
| Network            | Connectivity          | API slow/failing                 | HTTP response                 | `curl -v http://host:port`        | Response code/time         | 500 errors               | Fix app                         |                         |
| Network            | Bandwidth             | Network saturation               | Traffic usage                 | `iftop`                           | High bandwidth user        | Large file transfer      | Throttle                        |                         |
| DNS                | Name resolution       | Service unreachable              | DNS lookup                    | `nslookup`, `dig`                 | DNS resolution             | DNS timeout              | Fix DNS                         |                         |
| Processes          | App behavior          | Hung process                     | Process state                 | `ps -ef`, `top`                   | Running/stuck              | App stuck                | Kill/restart                    |                         |
| Processes          | App behavior          | Multiple instances               | Count processes               | `pgrep -l app`                    | Number of PIDs             | Too many workers         | Tune config                     |                         |
| Processes          | App behavior          | Graceful shutdown                | Signal handling               | `kill PID`                        | SIGTERM                    | Clean shutdown           | Preferred                       |                         |
| Processes          | App behavior          | Force stop                       | Immediate kill                | `kill -9 PID`                     | SIGKILL                    | Hung app                 | Last resort                     |                         |
| Orphan Process     | Parent exited         | Lingering process                | PPID check                    | `ps -ef                           | awk '$3==1'`               | Orphan PID               | Orphan Java process             | Kill or restart service |
| Zombie Process     | Not reaped            | Resource leak                    | Zombie state                  | `ps aux                           | grep Z`                    | Z state                  | Zombie present                  | Restart parent          |
| File Descriptors   | Open files            | “Too many open files”            | FD limits                     | `ulimit -n`                       | FD limit                   | App hits FD limit        | Increase limit                  |                         |
| File Descriptors   | Open files            | FD leak                          | Open FD list                  | `lsof -p PID`                     | FD usage                   | App leaking FDs          | Fix app                         |                         |
| Services           | Managed apps          | Service down                     | Service status                | `systemctl status app`            | Service state              | nginx stopped            | Restart                         |                         |
| Services           | Managed apps          | Frequent restarts                | Logs                          | `journalctl -u app`               | Error history              | Crash loop               | Root cause fix                  |                         |
| Logs               | Troubleshooting       | Unknown failure                  | System logs                   | `/var/log/messages`, `journalctl` | Error trace                | OOM error                | Memory tuning                   |                         |
| Cron Jobs          | Scheduled tasks       | Load spikes                      | Job schedule                  | `crontab -l`                      | Job timing                 | Backup at peak hours     | Reschedule                      |                         |
| Real Incident Flow | Troubleshooting order | Faster recovery                  | CPU → Mem → Disk → Net → Proc | `top → free → df → ss`            | Systematic approach        | App slow incident        | Fix bottleneck                  |                         |

🧠 Production Golden Rule
- When ANY incident happens, troubleshoot in this order:

```
CPU → Memory → Disk → Inodes → Network → Processes → Logs
```

## Linux Networking — Commands

| Area                    | What It Means         | Real Production Issue         | What to Check (Order) | Commands                                     | What the Command Shows | Real Example                | Fix / Action          |          |
| ----------------------- | --------------------- | ----------------------------- | --------------------- | -------------------------------------------- | ---------------------- | --------------------------- | --------------------- | -------- |
| Network Interface       | Physical/virtual NIC  | Server unreachable            | Interface up/down     | `ip a`, `nmcli dev status`                   | IP, state (UP/DOWN)    | Interface DOWN after reboot | `ip link set eth0 up` |          |
| IP Address              | System identity       | Wrong IP / no IP              | IP assignment         | `ip addr show`                               | Assigned IPs           | App bound to old IP         | Fix config / DHCP     |          |
| Routing                 | Path to destination   | Cannot reach external network | Default gateway       | `ip route`                                   | Routing table          | No default route            | Add gateway           |          |
| Connectivity            | Host reachability     | Host unreachable              | ICMP response         | `ping host`                                  | Packet loss, latency   | Packet loss 40%             | Network issue         |          |
| Latency                 | Network delay         | Slow API calls                | RTT time              | `ping`, `mtr`                                | Latency spikes         | RTT 400ms                   | Network tuning        |          |
| DNS                     | Name resolution       | App can’t resolve hostname    | DNS lookup            | `nslookup`, `dig`                            | DNS response           | DNS timeout                 | Fix resolv.conf       |          |
| Ports                   | Listening services    | App not reachable             | Open ports            | `ss -tulnp`                                  | Port + PID             | App not listening on 8080   | Restart app           |          |
| Firewall                | Traffic filtering     | Connection refused            | Firewall rules        | `iptables -L`, `firewall-cmd --list-all`     | Allowed/blocked ports  | Port blocked                | Allow port            |          |
| Service Binding         | App listening address | Works locally, not remotely   | Bind address          | `ss -tulnp`                                  | 127.0.0.1 vs 0.0.0.0   | Bound to localhost          | Bind to 0.0.0.0       |          |
| HTTP Testing            | App-level networking  | API returns error             | HTTP response         | `curl -v http://host:port`                   | Status code, latency   | 500 error                   | App fix               |          |
| Bandwidth               | Traffic volume        | Network saturation            | Traffic usage         | `iftop`, `nload`                             | High bandwidth users   | Backup flooding network     | Throttle / reschedule |          |
| Packet Drops            | Network reliability   | Random disconnects            | Errors/drops          | `ip -s link`                                 | RX/TX errors           | Dropped packets             | NIC / MTU fix         |          |
| MTU                     | Packet size           | VPN/network breakage          | MTU mismatch          | `ip link show`                               | MTU value              | VPN drops packets           | Lower MTU             |          |
| Established Connections | Client load           | Too many connections          | Connection count      | `ss -s`, `ss state established`              | Active connections     | 50k connections             | Tune app              |          |
| TIME_WAIT               | TCP cleanup           | Port exhaustion               | Socket states         | `ss -ant                                     | grep TIME-WAIT`        | TIME_WAIT count             | Port exhaustion       | Tune TCP |
| File Descriptors        | Socket limits         | “Too many open files”         | FD usage              | `ulimit -n`, `lsof -i`                       | FD/socket usage        | App hit FD limit            | Increase limit        |          |
| Reverse Proxy           | Traffic routing       | 502/504 errors                | Backend reachability  | `curl backend`, `ss -tulnp`                  | Backend status         | Backend down                | Restart backend       |          |
| Load Balancer           | Traffic distribution  | Uneven traffic                | Health checks         | `curl`, LB logs                              | Failed health checks   | One node overloaded         | Fix node              |          |
| SSL/TLS                 | Secure traffic        | HTTPS failure                 | Certificate           | `curl -vk https://host`                      | Cert, handshake        | Cert expired                | Renew cert            |          |
| Network Namespace       | Containers            | Pod can’t reach service       | Namespace config      | `ip netns`, `nsenter`                        | Namespace routing      | Pod isolated                | Fix CNI               |          |
| Docker Networking       | Container network     | Container unreachable         | Bridge/port mapping   | `docker ps`, `docker inspect`                | Port mappings          | Port not exposed            | Fix mapping           |          |
| Kubernetes Networking   | Pod/service comms     | Pod can’t reach service       | Service & endpoints   | `kubectl get svc,ep`                         | Service endpoints      | No endpoints                | Fix labels            |          |
| Logging                 | Network errors        | Random failures               | System logs           | `journalctl -u network`, `/var/log/messages` | Network errors         | NIC flapping                | Fix driver            |          |
| Monitoring              | Early detection       | No alerts                     | Metrics               | `sar -n DEV`, `netstat -i`                   | Traffic trends         | Gradual saturation          | Scale network         |          |
| Real Incident Flow      | End-to-end debugging  | App not reachable             | Layer-by-layer check  | Interface → IP → Route → DNS → Port → App    | Systematic isolation   | Prod outage                 | Root cause fixed      |          |


🧠 Golden Production Networking Troubleshooting Order
- When an app/network is broken, always check in this order:
- Interface → IP → Route → DNS → Port → Firewall → App → Logs

| Symptom                 | Root Cause             |
| ----------------------- | ---------------------- |
| Ping works, curl fails  | Port/firewall/app      |
| Curl works locally only | App bound to localhost |
| DNS fails, IP works     | DNS issue              |
| Random drops            | MTU / NIC / network    |
| 502/504 errors          | Backend down           |
| “Too many open files”   | FD/socket exhaustion   |


## Service Management

| Topic             | What It Is                               | Why It Matters in Production    | Commands                              | What the Command Does             | Real Production Issue          | Fix / Action             |
| ----------------- | ---------------------------------------- | ------------------------------- | ------------------------------------- | --------------------------------- | ------------------------------ | ------------------------ |
| Service           | Long-running background process (daemon) | Keeps apps running continuously | `nginx`, `mysql`, `sshd`              | Provides system/app functionality | Web app depends on nginx       | Service must be running  |
| systemd           | Init system (PID 1)                      | Manages boot & services         | `systemd`                             | Controls all services             | Server boot fails              | Fix systemd units        |
| systemctl         | Service management command               | Daily admin/SRE tool            | `systemctl status app`                | Shows state, PID, logs            | App down                       | Investigate status       |
| Service status    | Current service state                    | Health check                    | `systemctl status nginx`              | Active/inactive/failed            | nginx not responding           | Restart service          |
| Start service     | Start service manually                   | Bring app online                | `systemctl start app`                 | Starts service                    | App stopped accidentally       | Start service            |
| Stop service      | Stop service safely                      | Maintenance window              | `systemctl stop app`                  | Graceful stop                     | Planned downtime               | Stop safely              |
| Restart service   | Stop + start                             | Apply config changes            | `systemctl restart app`               | Full restart                      | Config updated                 | Restart app              |
| Reload service    | Reload config only                       | Zero-downtime changes           | `systemctl reload nginx`              | Reloads config                    | Config change without downtime | Prefer reload            |
| Enable service    | Start at boot                            | Ensure app survives reboot      | `systemctl enable app`                | Auto-start at boot                | App down after reboot          | Enable service           |
| Disable service   | Prevent auto-start                       | Security / cost control         | `systemctl disable app`               | Stops boot start                  | Unused service running         | Disable service          |
| Mask service      | Completely block service                 | Prevent accidental start        | `systemctl mask app`                  | Blocks start                      | Vulnerable service             | Mask service             |
| List services     | View all services                        | Auditing                        | `systemctl list-units --type=service` | Active services                   | Unknown services running       | Review list              |
| Service unit file | Service configuration                    | Controls behavior               | `/usr/lib/systemd/system/app.service` | Defines ExecStart, User           | Service fails to start         | Fix unit file            |
| Unit sections     | Service structure                        | Correct configuration           | `[Unit] [Service] [Install]`          | Dependencies & execution          | Wrong order at boot            | Fix dependencies         |
| Reload systemd    | Apply unit changes                       | Mandatory after edits           | `systemctl daemon-reload`             | Reloads unit files                | Changes ignored                | Run daemon-reload        |
| Service logs      | Runtime logs                             | Debug failures                  | `journalctl -u app`                   | Shows service logs                | App crashing                   | Analyze logs             |
| Live logs         | Real-time troubleshooting                | Debug production issues         | `journalctl -u app -f`                | Live log stream                   | App flapping                   | Monitor behavior         |
| Port conflict     | Service not starting                     | Port already in use             | `ss -tulnp`                           | Shows port + PID                  | Port 8080 busy                 | Kill conflicting process |
| Service crash     | Unexpected stop                          | App instability                 | `journalctl -xe`                      | System errors                     | App crash loop                 | Fix root cause           |
| Resource limit    | CPU/memory control                       | Prevent system overload         | `CPUQuota=50%`                        | Limits CPU                        | App hogging CPU                | Set limits               |
| Memory limit      | RAM control                              | Avoid OOM                       | `MemoryMax=1G`                        | Caps memory                       | App killed by OOM              | Tune memory              |
| File limit        | FD control                               | Prevent FD exhaustion           | `LimitNOFILE=65535`                   | Max open files                    | “Too many open files”          | Increase limit           |
| Security          | Non-root execution                       | Reduce attack surface           | `User=nginx`                          | Runs as non-root                  | Root compromise risk           | Use non-root user        |
| Dependency        | Startup order                            | Avoid race conditions           | `After=network.target`                | Start after network               | App fails at boot              | Fix dependency           |
| Auto-restart      | Self-healing                             | High availability               | `Restart=always`                      | Restarts on failure               | App crashes                    | Enable auto-restart      |
| Real scenario     | App down after reboot                    | Common prod issue               | `systemctl is-enabled app`            | Checks boot status                | App didn’t start               | Enable + restart         |
| Best practice     | Safe service ops                         | Stability & security            | Reload > restart                      | Less downtime                     | Production traffic             | Prefer reload            |

🧠 Production Golden Rule
- status → logs → ports → config → restart → enable


## 🔐 SELinux (Security-Enhanced Linux) — Single Master Table

| Topic                   | Explanation                                    | Why It Matters in Production                    | Commands                                      | What the Command Shows | Real Production Issue          | Fix / Best Practice       |
| ----------------------- | ---------------------------------------------- | ----------------------------------------------- | --------------------------------------------- | ---------------------- | ------------------------------ | ------------------------- |
| SELinux                 | Mandatory Access Control (MAC) security system | Protects system even if root/app is compromised | `sestatus`                                    | Current SELinux status | App denied access unexpectedly | Use proper context        |
| Purpose                 | Enforces security policies beyond file perms   | Prevent privilege escalation                    | —                                             | —                      | Hacker gains shell             | SELinux blocks damage     |
| MAC vs DAC              | SELinux overrides Linux permissions            | Root is restricted                              | —                                             | —                      | Root can’t access file         | SELinux policy applies    |
| Status                  | Enabled / Disabled                             | Security posture                                | `sestatus`                                    | Enforcing / Permissive | App not working                | Check mode                |
| Mode: Enforcing         | Blocks unauthorized access                     | Full protection                                 | `getenforce`                                  | Enforcing              | App denied                     | Fix policy                |
| Mode: Permissive        | Logs but allows access                         | Debugging mode                                  | `setenforce 0`                                | Permissive             | App works but logs denial      | Temporary troubleshooting |
| Mode: Disabled          | SELinux off                                    | Not recommended                                 | `/etc/selinux/config`                         | Disabled               | No protection                  | Avoid in prod             |
| Change mode (temp)      | Switch enforce/permissive                      | Debug quickly                                   | `setenforce 0/1`                              | Mode changed           | Debugging prod issue           | Use temporarily           |
| Change mode (permanent) | Persistent mode                                | Policy control                                  | `/etc/selinux/config`                         | Mode on reboot         | Server reboot                  | Set correctly             |
| Policy                  | Security rules                                 | Control allowed actions                         | `targeted`                                    | Active policy          | App blocked                    | Adjust policy             |
| Targeted policy         | Protects services                              | Default & safest                                | `sestatus`                                    | Targeted policy        | Web app blocked                | Fix context               |
| SELinux context         | Label on files/processes                       | Decision basis                                  | `ls -Z`                                       | Context labels         | App can’t read file            | Wrong label               |
| Context parts           | user:role:type:level                           | Access control                                  | —                                             | —                      | Type mismatch                  | Fix type                  |
| File context            | File security label                            | Controls access                                 | `ls -Z file`                                  | File label             | Web server can’t read file     | Relabel file              |
| Process context         | Process label                                  | Enforces runtime access                         | `ps -eZ`                                      | Process label          | App blocked                    | Check process type        |
| Common types            | `httpd_sys_content_t`                          | Web content access                              | —                                             | —                      | Nginx denied                   | Use correct type          |
| AVC denial              | SELinux violation log                          | Root cause data                                 | `ausearch -m avc`                             | Denial details         | Permission denied              | Analyze denial            |
| Audit logs              | SELinux events                                 | Debugging                                       | `/var/log/audit/audit.log`                    | All denials            | App failure                    | Inspect logs              |
| Allow rule generation   | Create custom policy                           | Permanent fix                                   | `audit2allow`                                 | Policy suggestion      | Repeated denials               | Apply policy              |
| Temporary allow         | Boolean toggle                                 | Safe feature enable                             | `getsebool`                                   | Boolean status         | Web needs DB                   | Enable boolean            |
| Set boolean             | Enable feature                                 | No policy edit                                  | `setsebool -P httpd_can_network_connect on`   | Boolean enabled        | Web app DB connect             | Best practice             |
| Relabel file            | Fix wrong label                                | Common prod fix                                 | `restorecon -v file`                          | Correct context        | App denied file                | Relabel                   |
| Persistent context      | Survive reboot                                 | Prevent relabel loss                            | `semanage fcontext`                           | Custom context         | Files reset after reboot       | Use semanage              |
| Port labeling           | Allow service ports                            | Non-standard ports                              | `semanage port -l`                            | Allowed ports          | App port blocked               | Add port                  |
| Add port                | Allow service port                             | Custom ports                                    | `semanage port -a -t http_port_t -p tcp 8080` | Port allowed           | Nginx on 8080 fails            | Fix port context          |
| Disable SELinux (bad)   | Turn off security                              | Dangerous                                       | `setenforce 0`                                | Permissive             | App works                      | NOT recommended           |
| Best practice           | Fix policy, not disable                        | Secure prod                                     | —                                             | —                      | Frequent denials               | Proper config             |
| Common mistake          | Disabling SELinux                              | Security risk                                   | —                                             | —                      | Breach impact high             | Keep enforcing            |
| Real scenario           | Web app can’t connect DB                       | SELinux blocks network                          | `setsebool httpd_can_network_connect on`      | Access allowed         | 502 errors                     | Boolean fix               |
| Real scenario           | App can’t read files                           | Wrong context                                   | `restorecon -Rv /var/www`                     | Access restored        | 403 errors                     | Relabel                   |
| Real scenario           | App on custom port                             | Port not labeled                                | `semanage port -a`                            | Port allowed           | Service down                   | Label port                |
| Troubleshooting flow    | Step-by-step                                   | Faster resolution                               | `getenforce → ausearch → restorecon`          | Root cause             | Prod outage                    | Correct fix               |
| Interview key           | Why SELinux                                    | Defense in depth                                | —                                             | —                      | Root compromised               | SELinux limits damage     |


## 📦 Linux Repository Management & Patching

| Step | What You Do (Process)       | YUM / DNF Command (Example)                                      | What This Does            | If Something Goes Wrong       | Likely Cause          | How to Fix (Prod Way)   |                   |
| ---- | --------------------------- | ---------------------------------------------------------------- | ------------------------- | ----------------------------- | --------------------- | ----------------------- | ----------------- |
| 1    | Check OS & version          | `cat /etc/redhat-release`                                        | Confirms OS compatibility | Packages not found            | Wrong repo for OS     | Use correct OS repo     |                   |
| 2    | Check network               | `ping google.com`                                                | Confirms connectivity     | Repo unreachable              | DNS / firewall issue  | Fix DNS / firewall      |                   |
| 3    | List enabled repos          | `yum repolist`<br>`dnf repolist`                                 | Shows active repos        | Repo missing                  | Repo disabled         | Enable repo             |                   |
| 4    | List all repos              | `yum repolist all`<br>`dnf repolist all`                         | Shows enabled + disabled  | Repo disabled                 | Config issue          | Enable repo             |                   |
| 5    | Check repo files            | `ls /etc/yum.repos.d/`                                           | Repo config location      | Repo file missing             | Repo not added        | Add repo file           |                   |
| 6    | Add custom repo             | `dnf config-manager --add-repo http://repo.example.com/app.repo` | Adds repo                 | Repo not added                | Bad URL               | Fix URL                 |                   |
| 7    | Enable repo                 | `dnf config-manager --set-enabled apprepo`                       | Enables repo              | Still disabled                | Typo in repo ID       | Check repo ID           |                   |
| 8    | Clean metadata              | `yum clean all`<br>`dnf clean all`                               | Clears cached data        | Old metadata                  | Cache corruption      | Always clean cache      |                   |
| 9    | Check updates               | `yum check-update`<br>`dnf check-update`                         | Lists pending updates     | Command hangs                 | Repo slow/down        | Fix repo / mirror       |                   |
| 10   | Review what will update     | `dnf update --assumeno`                                          | Shows changes only        | Unexpected packages           | Repo mismatch         | Review repos            |                   |
| 11   | Apply security patches only | `dnf update --security`                                          | Applies CVE fixes         | No security updates           | Plugin missing        | Install security plugin |                   |
| 12   | Patch single package        | `dnf update openssl`                                             | Updates only openssl      | Dependency conflict           | Version mismatch      | Use `--allowerasing`    |                   |
| 13   | Apply all patches           | `dnf update -y`                                                  | Updates system            | App breaks                    | App incompatibility   | Rollback / restart      |                   |
| 14   | Kernel patch                | `dnf update kernel`                                              | Updates kernel            | Kernel installed but inactive | Reboot needed         | Schedule reboot         |                   |
| 15   | Check running kernel        | `uname -r`                                                       | Shows active kernel       | Old kernel running            | No reboot             | Reboot system           |                   |
| 16   | Reboot system               | `reboot`                                                         | Activates kernel          | Server not coming up          | Boot issue            | Use old kernel          |                   |
| 17   | Verify services             | `systemctl status nginx`                                         | Confirms services         | Service failed                | Config / SELinux      | Check logs              |                   |
| 18   | Check service logs          | `journalctl -u nginx`                                            | Shows failure reason      | Permission denied             | SELinux               | Fix context             |                   |
| 19   | Check disk space            | `df -h`                                                          | Disk availability         | Update failed                 | Disk full             | Cleanup logs            |                   |
| 20   | Check inodes                | `df -i`                                                          | File metadata             | Disk free but error           | Inodes full           | Delete small files      |                   |
| 21   | Check GPG key               | Error: `Public key not installed`                                | Security validation       | GPG key missing               | Key not imported      | `rpm --import KEY`      |                   |
| 22   | Dependency conflict         | Error: `conflicting requests`                                    | Package clash             | Mixed repos                   | Repo inconsistency    | Use correct repos       |                   |
| 23   | Force dependency resolve    | `dnf update --allowerasing`                                      | Resolves conflicts        | Removes packages              | Risky removal         | Review carefully        |                   |
| 24   | View patch history          | `dnf history`                                                    | Shows patch history       | Unknown change                | Auto update           | Audit history           |                   |
| 25   | Rollback bad patch          | `dnf history undo ID`                                            | Reverts RPM change        | Kernel rollback               | Kernel can’t rollback | Boot old kernel         |                   |
| 26   | Verify versions             | `rpm -qa                                                         | grep openssl`             | Confirms version              | Wrong version         | Repo priority           | Fix repo priority |
| 27   | Enable auto patching        | `dnf install dnf-automatic`                                      | Enables automation        | Unexpected updates            | Kernel auto update    | Disable kernel auto     |                   |
| 28   | Schedule auto patch         | `systemctl enable dnf-automatic.timer`                           | Schedules updates         | Prod outage                   | Bad timing            | Use maintenance window  |                   |
| 29   | Prod best practice          | —                                                                | Patch staging first       | Prod outage                   | No testing            | Always stage first      |                   |
| 30   | Final verification          | App test / curl                                                  | Confirms app health       | App broken                    | Patch impact          | Rollback / fix          |                   |

🧠 REAL PRODUCTION PATCHING FLOW (MEMORIZE THIS)
- Check → Review → Stage → Backup → Patch → Reboot → Verify → Monitor

# Linux Production Issues

| Category          | Production Problem        | Real Production Example    | Troubleshooting Commands        | Root Cause             | Fix / Resolution        |                |
| ----------------- | ------------------------- | -------------------------- | ------------------------------- | ---------------------- | ----------------------- | -------------- |
| **CPU**           | High CPU usage            | Java app response time >5s | `top`, `uptime`                 | CPU saturation         | Scale CPU / restart app |                |
| CPU               | Load high but CPU idle    | Load 12 on 4-core server   | `uptime`, `iostat`              | High IO wait           | Move to SSD             |                |
| CPU               | One core maxed            | Nginx single worker 100%   | `mpstat -P ALL`                 | App not multi-threaded | Increase workers        |                |
| CPU               | Backup job slowing prod   | Night backup impacts users | `top`, `ps -eo pid,ni,cmd`      | No nice value          | `renice 10 PID`         |                |
| **Memory**        | App killed suddenly       | App logs show “Killed”     | `dmesg \| grep oom`             | OOM Killer             | Increase RAM            |                |
| Memory            | Heavy swap usage          | System very slow           | `free -h`, `vmstat 1`           | RAM exhausted          | Tune app                |                |
| Memory            | Memory leak               | Java heap grows daily      | `ps aux --sort=-%mem`           | App leak               | Restart + fix           |                |
| Memory            | Cache using RAM           | Free memory low            | `free -h`                       | Linux cache            | Normal behavior         |                |
| **Disk**          | Disk full                 | App fails writing logs     | `df -h`                         | Logs filled disk       | Cleanup logs            |                |
| Disk              | Space not freed           | Disk still 100%            | `lsof \| grep deleted`          | Open deleted file      | Restart app             |                |
| Disk              | Slow DB queries           | DB latency spikes          | `iostat -xz`, `iotop`           | Disk bottleneck        | Faster disk             |                |
| Disk              | Root FS full              | SSH login fails            | `df -h /`                       | /var logs              | Cleanup `/var`          |                |
| **Inodes**        | Disk free but write fails | “No space left”            | `df -i`                         | Inodes exhausted       | Delete files            |                |
| Inodes            | Temp files explosion      | /tmp millions files        | `find /tmp -type f              | wc -l`                 | App bug                 | Cleanup        |
| **Network**       | Server unreachable        | SSH timeout                | `ping`, `ip a`                  | Network down           | Fix NIC                 |                |
| Network           | DNS not resolving         | App works with IP only     | `nslookup app.com`              | DNS misconfig          | Fix resolver            |                |
| Network           | App local only            | Curl localhost works       | `ss -tulnp`                     | Bound to 127.0.0.1     | Bind 0.0.0.0            |                |
| Network           | Connection refused        | Immediate failure          | `ss -tulnp`                     | App not running        | Restart service         |                |
| Network           | Random drops              | SSH disconnects            | `ping`, `mtr`                   | MTU issue              | Fix MTU                 |                |
| Network           | Firewall block            | Timeout from users         | `firewall-cmd --list-all`       | Port blocked           | Open port               |                |
| **Services**      | Service not starting      | nginx failed               | `systemctl status nginx`        | Config error           | Fix config              |                |
| Services          | Crash loop                | App restarts every 5s      | `journalctl -u app`             | App bug                | Fix app                 |                |
| Services          | App down after reboot     | Server rebooted            | `systemctl is-enabled app`      | Not enabled            | Enable service          |                |
| Services          | Port conflict             | Service won’t start        | `ss -tulnp`                     | Port already used      | Kill conflict           |                |
| Services          | Slow boot                 | Server takes 5 min         | `systemd-analyze blame`         | Bad service            | Disable                 |                |
| **Processes**     | Hung process              | App frozen                 | `ps -ef`, `top`                 | Deadlock               | Kill PID                |                |
| Processes         | Zombie processes          | ps shows Z state           | `ps aux                         | grep Z`                | Parent bug              | Restart parent |
| Processes         | Orphan process            | Unknown PID running        | `ps -ef                         | awk '$3==1'`           | Parent died             | Kill PID       |
| Processes         | Too many threads          | CPU spike                  | `ps -eLf                        | wc -l`                 | Thread leak             | Fix app        |
| **Files & Perms** | Permission denied         | App can’t read config      | `ls -l`, `id`                   | Wrong ownership        | `chown`                 |                |
| Files             | Wrong umask               | Files inaccessible         | `umask`                         | Bad umask              | Fix profile             |                |
| Files             | FS read-only              | Write fails                | `mount`, `dmesg`                | FS error               | fsck                    |                |
| **SELinux**       | App works when disabled   | Works only in permissive   | `getenforce`, `ausearch -m avc` | Wrong context          | `restorecon`            |                |
| SELinux           | Web → DB blocked          | 502 errors                 | `getsebool -a`                  | Boolean off            | Enable boolean          |                |
| SELinux           | Custom port blocked       | App won’t bind             | `semanage port -l`              | Port unlabeled         | Add port                |                |
| SELinux           | Context resets            | Issue returns              | `ls -Z`                         | Not persistent         | `semanage fcontext`     |                |
| **Patching**      | Repo unreachable          | dnf fails                  | `dnf repolist`                  | DNS/firewall           | Fix network             |                |
| Patching          | GPG error                 | Install blocked            | Error output                    | Missing key            | Import key              |                |
| Patching          | Dependency conflict       | Update fails               | `dnf update --allowerasing`     | Mixed repos            | Fix repos               |                |
| Patching          | Kernel patched but vuln   | CVE alert                  | `uname -r`                      | No reboot              | Reboot                  |                |
| Patching          | App broke after patch     | Prod outage                | `dnf history`                   | Incompatible update    | Rollback                |                |
| **Cron**          | Cron job not running      | Backup didn’t run          | `crontab -l`, logs              | Wrong env              | Use full path           |                |
| Cron              | Script works manually     | Fails in cron              | Cron logs                       | PATH issue             | Export PATH             |                |
| **Logs**          | Logs not rotating         | Disk fills nightly         | `ls /etc/logrotate.d`           | No rule                | Add logrotate           |                |
| Logs              | No logs                   | Blind outage               | `journalctl`                    | Logging off            | Enable logs             |                |
| **Time**          | Clock drift               | TLS handshake fails        | `timedatectl`                   | NTP disabled           | Enable chrony           |                |
| **Limits**        | Too many open files       | App crash                  | `ulimit -n`, `lsof`             | FD limit               | Increase limit          |                |
| Limits            | Process limit hit         | SSH login fails            | `ulimit -u`                     | Low proc limit         | Fix limits.conf         |                |
| **Boot**          | Kernel panic              | Server won’t boot          | GRUB menu                       | Bad kernel             | Boot old kernel         |                |
| Boot              | FS corruption             | Drops to rescue            | `fsck`                          | Disk error             | Repair FS               |                |

### 🧠 REAL PRODUCTION DEBUG ORDER
```
Service
→ Logs
→ CPU
→ Memory
→ Disk
→ Inodes
→ Network
→ SELinux
→ Patching
→ Reboot (last resort)
```

# ☸️ Kubernetes Production Issues

| Category        | Production Issue         | Real Production Example       | Troubleshooting Commands            | Root Cause               | Fix Command(s)                                                  |
| --------------- | ------------------------ | ----------------------------- | ----------------------------------- | ------------------------ | --------------------------------------------------------------- |
| **Pods**        | Pod Pending              | New release pod never starts  | `kubectl describe pod app-pod`      | No CPU/memory available  | Scale nodes or reduce requests                                  |
| Pods            | CrashLoopBackOff         | App restarts every 10 seconds | `kubectl logs app-pod`              | App crash / bad config   | Fix app → `kubectl rollout restart deploy app`                  |
| Pods            | ImagePullBackOff         | Pod stuck pulling image       | `kubectl describe pod app-pod`      | Wrong image name/tag     | `kubectl set image deploy app app=image:v2`                     |
| Pods            | OOMKilled                | Pod restarts under load       | `kubectl describe pod app-pod`      | Memory limit too low     | Increase memory limit in deployment                             |
| Pods            | Init container stuck     | DB init never completes       | `kubectl logs pod -c init-db`       | Init script error        | Fix init script & redeploy                                      |
| Pods            | Pod stuck Terminating    | Pod never deletes             | `kubectl get pod app-pod -o yaml`   | Finalizer stuck          | `kubectl patch pod app-pod -p '{"metadata":{"finalizers":[]}}'` |
| Pods            | Pod evicted              | Pod restarted suddenly        | `kubectl describe pod`              | Node pressure            | Free node resources                                             |
| **Nodes**       | Node NotReady            | Pods evicted                  | `kubectl describe node node1`       | Kubelet down             | `systemctl restart kubelet`                                     |
| Nodes           | DiskPressure             | Pods evicted                  | `kubectl describe node node1`       | Node disk full           | Cleanup node disk                                               |
| Nodes           | MemoryPressure           | Pods evicted                  | `kubectl describe node node1`       | Node RAM exhausted       | Scale node pool                                                 |
| Nodes           | NetworkUnavailable       | Pods unreachable              | `kubectl get events -A`             | CNI plugin down          | Restart CNI pods                                                |
| Nodes           | Clock skew               | TLS errors                    | `timedatectl` (node)                | NTP disabled             | Enable NTP                                                      |
| **Scheduling**  | NodeSelector mismatch    | Pod Pending                   | `kubectl describe pod`              | Label mismatch           | Fix node labels                                                 |
| Scheduling      | Affinity too strict      | Pod Pending                   | `kubectl describe pod`              | Hard affinity rules      | Relax affinity                                                  |
| Scheduling      | Missing toleration       | Pod Pending                   | `kubectl describe pod`              | Node taint               | Add toleration                                                  |
| **Networking**  | Service unreachable      | API timeout                   | `kubectl get svc,ep`                | No endpoints             | Fix pod labels                                                  |
| Networking      | Pod can’t reach service  | Connection refused            | `kubectl exec pod -- curl svc`      | CNI issue                | Restart CNI                                                     |
| Networking      | DNS not resolving        | App fails by name             | `kubectl exec pod -- nslookup svc`  | CoreDNS down             | Restart CoreDNS                                                 |
| Networking      | NetworkPolicy blocking   | Timeout                       | `kubectl describe netpol`           | Policy deny              | Update NetworkPolicy                                            |
| **Services**    | Service has no endpoints | App down                      | `kubectl get endpoints`             | Selector mismatch        | Fix service selector                                            |
| Services        | NodePort not reachable   | External access fails         | `kubectl describe svc`              | Firewall closed          | Open node firewall                                              |
| **Ingress**     | Ingress 404              | Traffic fails                 | `kubectl describe ingress`          | Wrong path/host          | Fix ingress rules                                               |
| Ingress         | Ingress 502/504          | Gateway error                 | Ingress logs                        | Backend pod down         | Fix backend                                                     |
| Ingress         | TLS not working          | HTTPS fails                   | `kubectl describe ingress`          | Cert missing/expired     | Update TLS secret                                               |
| **Storage**     | PVC Pending              | Pod stuck                     | `kubectl describe pvc`              | No PV                    | Create PV                                                       |
| Storage         | Volume mount timeout     | Pod stuck                     | Pod events                          | CSI backend down         | Fix storage backend                                             |
| Storage         | Permission denied        | App crash                     | `kubectl logs pod`                  | FS/SELinux perms         | Fix ownership/context                                           |
| Storage         | Disk full                | Pod evicted                   | `kubectl describe node`             | Node disk full           | Cleanup disk                                                    |
| **ConfigMaps**  | Config not updated       | App using old config          | `kubectl describe pod`              | Pod not restarted        | Restart deployment                                              |
| ConfigMaps      | App crashes on start     | CrashLoopBackOff              | `kubectl logs pod`                  | Missing config key       | Fix ConfigMap                                                   |
| **Secrets**     | App won’t start          | Crash on boot                 | `kubectl describe pod`              | Secret missing           | Create/update secret                                            |
| Secrets         | Auth failure             | DB login fails                | App logs                            | Wrong secret value       | Update secret                                                   |
| **Security**    | Pod forbidden            | Pod rejected                  | `kubectl describe pod`              | PodSecurity rules        | Fix securityContext                                             |
| Security        | Privileged pod blocked   | Pod fails admission           | Pod events                          | Policy restriction       | Adjust policy                                                   |
| Security        | RunAsNonRoot error       | Pod won’t start               | Pod events                          | SecurityContext mismatch | Fix user config                                                 |
| **Deployments** | Rollout stuck            | Deploy hangs                  | `kubectl rollout status deploy app` | Pod crash                | Fix image                                                       |
| Deployments     | Bad deploy               | App broken                    | `kubectl rollout history`           | Bad revision             | Rollback                                                        |
| Deployments     | Rollback needed          | Prod outage                   | `kubectl rollout undo deploy app`   | Bad image                | Rollback                                                        |
| Deployments     | Old image running        | Bug persists                  | `kubectl describe pod`              | Mutable tag              | Use versioned tags                                              |
| **Scaling**     | HPA not scaling          | High latency                  | `kubectl describe hpa`              | Metrics missing          | Fix metrics-server                                              |
| Scaling         | Too many pods            | High cost                     | `kubectl get hpa`                   | Aggressive thresholds    | Tune HPA                                                        |
| **Performance** | CPU throttling           | App slow                      | `kubectl describe pod`              | Low CPU limit            | Increase CPU limit                                              |
| Performance     | High latency             | API slow                      | `kubectl top pod`                   | Resource starvation      | Increase requests                                               |
| **Cluster**     | API server slow          | kubectl hangs                 | Control plane logs                  | etcd overload            | Scale control plane                                             |
| Cluster         | etcd disk full           | Cluster unstable              | etcd metrics                        | No compaction            | Compact etcd                                                    |
| **Events**      | Silent failure           | No logs                       | `kubectl get events -A`             | Missed event             | Fix root cause                                                  |

🧠 KUBERNETES TROUBLESHOOTING ORDER
```
Pod
→ Logs
→ Events
→ Node
→ Network
→ Storage
→ Security
→ Deployment / Rollout
```

# 🔥 Docker mega table with examples & fix commands

| Category          | Production Issue            | Real Production Example               | Troubleshooting Commands          | Root Cause                    | Fix Command(s)                      |
| ----------------- | --------------------------- | ------------------------------------- | --------------------------------- | ----------------------------- | ----------------------------------- |
| **Containers**    | Container exits immediately | App container stops right after start | `docker ps -a`, `docker logs app` | App crash / bad CMD           | Fix CMD/ENTRYPOINT → `docker run …` |
| Containers        | Restart loop                | Container restarts every few seconds  | `docker ps`, `docker logs`        | App failure / bad healthcheck | Fix app or healthcheck              |
| Containers        | Container stuck in Created  | Container never runs                  | `docker inspect container`        | Volume / permission issue     | Fix volume perms                    |
| Containers        | Container stuck in Exited   | App not running                       | `docker logs container`           | Runtime error                 | Fix app                             |
| **Images**        | Image pull fails            | `pull access denied`                  | `docker pull image`               | Wrong image name / auth       | Login / fix image name              |
| Images            | Image not found             | CI build fails                        | `docker pull image:tag`           | Wrong tag                     | Use correct tag                     |
| Images            | Old image used              | Bug persists after deploy             | `docker images`, `docker inspect` | Cached image                  | Pull latest image                   |
| Images            | Large image size            | Slow deploy                           | `docker images`                   | Bloated Dockerfile            | Use multi-stage build               |
| **Networking**    | Container not reachable     | App works inside container only       | `docker ps`, `docker inspect`     | Port not exposed              | Map port correctly                  |
| Networking        | Wrong port mapping          | Users can’t access app                | `docker inspect`                  | Wrong `-p` flag               | Fix port mapping                    |
| Networking        | Container DNS not working   | Can’t reach other containers          | `docker network inspect`          | Network misconfig             | Use correct network                 |
| Networking        | Containers can’t talk       | Microservices fail                    | `docker network ls`               | Different networks            | Attach to same network              |
| Networking        | Random disconnects          | API failures                          | `docker stats`                    | Resource pressure             | Tune resources                      |
| **Volumes**       | Permission denied           | App can’t write to volume             | `ls -l`, `ls -Z`                  | Wrong ownership/SELinux       | Fix ownership/context               |
| Volumes           | Data lost after restart     | App data disappears                   | `docker inspect`                  | No volume used                | Use named volume                    |
| Volumes           | Volume mount fails          | Container won’t start                 | `docker inspect`, logs            | Path not exist                | Create host dir                     |
| Volumes           | Read-only FS error          | Write fails                           | Logs                              | Volume mounted RO             | Remove `:ro`                        |
| **Resources**     | High CPU usage              | Host slow                             | `docker stats`                    | No CPU limit                  | Set CPU limits                      |
| Resources         | High memory usage           | OOM kill                              | `docker stats`, logs              | No memory limit               | Set memory limit                    |
| Resources         | Container killed            | “OOMKilled”                           | `docker inspect`                  | Low memory limit              | Increase memory                     |
| Resources         | Disk full                   | Docker stops                          | `docker system df`                | Old images/logs               | Cleanup Docker                      |
| **Docker Engine** | Docker daemon down          | All containers stopped                | `systemctl status docker`         | Docker crash                  | Restart Docker                      |
| Engine            | Docker won’t start          | Service fails                         | `journalctl -u docker`            | Config error                  | Fix daemon.json                     |
| Engine            | Docker slow                 | Builds slow                           | `docker info`                     | Disk/overlay issue            | Cleanup / upgrade                   |
| Engine            | OverlayFS issue             | Container errors                      | Docker logs                       | FS corruption                 | Restart daemon                      |
| **Builds**        | Build fails                 | CI build error                        | `docker build .`                  | Dockerfile error              | Fix Dockerfile                      |
| Builds            | Build slow                  | Long CI time                          | Build logs                        | No cache                      | Optimize layers                     |
| Builds            | Dependency download slow    | Build timeout                         | Logs                              | Network issue                 | Use cache/proxy                     |
| Builds            | Build works locally not CI  | CI fails                              | CI logs                           | Missing ARG/ENV               | Add ARG/ENV                         |
| **Healthchecks**  | Container unhealthy         | LB removes container                  | `docker inspect`                  | Bad healthcheck               | Fix healthcheck                     |
| Healthchecks      | Healthcheck never healthy   | App marked unhealthy                  | Logs                              | Wrong endpoint                | Fix check                           |
| **Security**      | Container running as root   | Security risk                         | `docker inspect`                  | Default root user             | Use non-root user                   |
| Security          | Secrets exposed             | Password in logs                      | CI logs                           | Secrets in ENV                | Use Docker secrets                  |
| Security          | Image vulnerability         | Security scan fails                   | Scanner output                    | Old base image                | Update base image                   |
| **Logging**       | Logs filling disk           | Node disk full                        | `du -sh /var/lib/docker`          | No log rotation               | Configure logging                   |
| Logging           | No logs available           | Debug impossible                      | `docker logs`                     | Logging disabled              | Enable logging                      |
| Logging           | Logs too verbose            | Disk growth                           | Logs                              | Debug enabled                 | Reduce log level                    |
| **Cleanup**       | Disk full                   | Docker stops pulling                  | `docker system df`                | Unused images                 | Prune Docker                        |
| Cleanup           | Old containers              | Node cluttered                        | `docker ps -a`                    | No cleanup                    | Remove containers                   |
| Cleanup           | Dangling images             | Disk wasted                           | `docker images -f dangling=true`  | Build leftovers               | Remove images                       |
| **CI/CD**         | Build succeeds, run fails   | Prod outage                           | `docker logs`                     | Runtime env diff              | Fix runtime config                  |
| CI/CD             | Wrong image deployed        | Old bug persists                      | `docker inspect`                  | Tag reused                    | Use versioned tags                  |
| CI/CD             | Registry auth fails         | Pipeline fails                        | CI logs                           | Missing creds                 | Login to registry                   |

🧠 DOCKER TROUBLESHOOTING ORDER
```
docker ps
→ docker logs
→ docker inspect
→ docker stats
→ docker system df
→ Docker daemon logs
```

# 🌍 Terraform Production Issues

| Category           | Production Issue              | Real Production Example   | Troubleshooting Commands           | Root Cause              | Fix Command(s)              |
| ------------------ | ----------------------------- | ------------------------- | ---------------------------------- | ----------------------- | --------------------------- |
| **Init**           | `terraform init` fails        | New infra setup fails     | `terraform init -reconfigure`      | Wrong backend/provider  | Fix backend/provider config |
| Init               | Provider download fails       | Init stuck at provider    | `terraform init -upgrade`          | Network / proxy issue   | Fix proxy / mirror          |
| Init               | Backend auth error            | State backend unreachable | Error output                       | Wrong credentials       | Fix backend creds           |
| **Plan**           | Plan shows unexpected changes | Infra drift alert         | `terraform plan`                   | Manual change in cloud  | Re-apply or import          |
| Plan               | Plan fails validation         | CI pipeline fails         | `terraform validate`               | Syntax error            | Fix HCL syntax              |
| Plan               | Variables missing             | Plan fails                | Error output                       | Missing tfvars          | Provide variables           |
| Plan               | Wrong resources planned       | Prod resources changed    | `terraform plan -out=plan`         | Wrong workspace         | Switch workspace            |
| **Apply**          | Apply fails mid-way           | Partial infra created     | `terraform apply`                  | API error               | Fix issue & re-apply        |
| Apply              | Apply hangs                   | No progress               | `terraform apply -lock-timeout=5m` | Provider/API hang       | Retry / upgrade provider    |
| Apply              | Permission denied             | Apply fails               | Error logs                         | IAM role missing perms  | Fix IAM policy              |
| Apply              | Rate limit exceeded           | AWS throttling            | Error logs                         | Too many API calls      | Add retries / slow down     |
| **State**          | State locked                  | Pipeline blocked          | Error: state lock                  | Previous run crashed    | Force unlock                |
| State              | State file corrupted          | Apply fails               | Error output                       | Manual edit             | Restore backup              |
| State              | Resource drift                | Infra mismatch            | `terraform plan`                   | Manual cloud change     | Reconcile state             |
| State              | Wrong resource in state       | Terraform wants recreate  | `terraform state list`             | Bad import              | Fix state                   |
| State              | Lost state file               | Infra unmanaged           | No state                           | Deleted backend         | Restore from backup         |
| **Import**         | Resource exists error         | Apply fails               | Error output                       | Resource already exists | Import resource             |
| Import             | Wrong import ID               | Import fails              | Error output                       | Wrong resource ID       | Fix ID                      |
| **Destroy**        | Destroy blocked               | Infra not deleting        | Error output                       | Dependency issue        | Fix dependency              |
| Destroy            | Accidentally destroying prod  | Panic                     | `terraform plan`                   | Wrong workspace         | Switch workspace            |
| Destroy            | Partial destroy               | Leftover resources        | Cloud console                      | Dependency graph        | Manual cleanup              |
| **Providers**      | Provider crash                | Apply fails               | Error logs                         | Buggy provider          | Upgrade provider            |
| Providers          | Version conflict              | Init fails                | Error output                       | Version constraint      | Align versions              |
| Providers          | Deprecated argument           | Plan warning/fail         | Plan output                        | Old syntax              | Update code                 |
| **Modules**        | Module not found              | Init fails                | Error output                       | Wrong source path       | Fix module source           |
| Modules            | Module version mismatch       | Unexpected changes        | Plan output                        | Wrong version           | Pin module version          |
| Modules            | Breaking module upgrade       | Infra broken              | Apply output                       | Unreviewed upgrade      | Rollback module             |
| **Variables**      | Wrong values applied          | Wrong infra size          | Plan output                        | Wrong tfvars            | Fix tfvars                  |
| Variables          | Sensitive value exposed       | Secrets in logs           | CI logs                            | Not marked sensitive    | Mark sensitive              |
| **Workspaces**     | Wrong env modified            | Prod impacted             | `terraform workspace show`         | Wrong workspace         | Switch workspace            |
| Workspaces         | Workspace missing             | Apply fails               | Error output                       | Not created             | Create workspace            |
| **Remote Backend** | Backend unreachable           | Apply fails               | Error output                       | Network/DNS issue       | Fix connectivity            |
| Backend            | S3 backend access denied      | State error               | Error logs                         | IAM missing perms       | Fix IAM                     |
| Backend            | DynamoDB lock stuck           | Apply blocked             | Lock error                         | Stale lock              | Force unlock                |
| **CI/CD**          | Works locally, fails in CI    | Pipeline red              | CI logs                            | Missing vars/creds      | Add CI variables            |
| CI/CD              | Secrets exposed in logs       | Security incident         | CI logs                            | Plain vars              | Use secret store            |
| CI/CD              | Plan OK, apply fails          | Prod outage               | Apply logs                         | Different env           | Align env                   |
| **Security**       | Hardcoded secrets             | Security risk             | Code review                        | Bad practice            | Use Vault/Secrets           |
| Security           | Public resources created      | Audit alert               | Plan output                        | Open SG rules           | Restrict rules              |
| **Performance**    | Terraform slow                | Long plan/apply           | `terraform providers lock`         | Large state             | Split modules               |
| Performance        | Large blast radius            | Too many changes          | Plan output                        | Monolithic config       | Break into modules          |
| **Recovery**       | Bad apply in prod             | Outage                    | `terraform history`                | Bad change              | Rollback via state          |
| Recovery           | Need emergency fix            | Infra down                | Console + plan                     | Terraform slow          | Manual fix + import         |

🧠 TERRAFORM TROUBLESHOOTING ORDER
```
terraform init
→ terraform validate
→ terraform plan
→ terraform apply
→ terraform state
→ Backend / IAM
```

# ⚙️ Ansible Production Issues

| Category           | Production Issue              | Real Production Example         | Troubleshooting Commands                   | Root Cause              | Fix Command(s)         |
| ------------------ | ----------------------------- | ------------------------------- | ------------------------------------------ | ----------------------- | ---------------------- |
| **Inventory**      | Host unreachable              | Playbook fails on some servers  | `ansible all -m ping`                      | SSH/DNS issue           | Fix SSH/DNS            |
| Inventory          | Wrong hosts targeted          | Prod servers updated by mistake | `ansible-inventory --graph`                | Wrong inventory group   | Fix inventory file     |
| Inventory          | Dynamic inventory fails       | No hosts found                  | Inventory logs                             | Cloud API/creds issue   | Fix credentials        |
| Inventory          | Inventory not updated         | New servers ignored             | `ansible-inventory --list`                 | Cache issue             | Refresh inventory      |
| **SSH / Auth**     | SSH auth failure              | UNREACHABLE error               | `ansible all -m ping -vvv`                 | Missing SSH key         | Copy SSH key           |
| SSH                | Wrong SSH user                | Permission denied               | `ansible.cfg`                              | Wrong `remote_user`     | Set correct user       |
| SSH                | SSH timeout                   | Tasks hang                      | `ssh user@host`                            | Network/firewall        | Fix network            |
| **Playbooks**      | Syntax error                  | Playbook fails immediately      | `ansible-playbook play.yml --syntax-check` | YAML error              | Fix YAML               |
| Playbooks          | Playbook slow                 | Long execution time             | `ansible-playbook -vvv`                    | Serial execution        | Increase forks         |
| Playbooks          | Task skipped unexpectedly     | App not installed               | Play output                                | Wrong `when` condition  | Fix condition          |
| Playbooks          | Wrong task order              | App fails to start              | Play logs                                  | Dependency issue        | Reorder tasks          |
| **Modules**        | Module not found              | Task fails                      | Error output                               | Missing collection      | Install collection     |
| Modules            | Deprecated module             | Playbook warning                | Play output                                | Old syntax              | Update module          |
| Modules            | Command vs shell misuse       | Script fails                    | Error logs                                 | Wrong module            | Use correct module     |
| **Variables**      | Variable undefined            | Playbook fails                  | Error output                               | Missing vars            | Define variable        |
| Variables          | Wrong var precedence          | Wrong config applied            | Debug output                               | Var override            | Fix precedence         |
| Variables          | Secrets exposed               | Password in logs                | CI logs                                    | No vault                | Use Ansible Vault      |
| **Facts**          | Wrong facts used              | Wrong OS task                   | `ansible all -m setup`                     | Cached facts            | Refresh facts          |
| Facts              | Facts gathering slow          | Playbook slow                   | Play output                                | Too many facts          | Disable facts          |
| **Roles**          | Role not found                | Playbook fails                  | Error output                               | Wrong path              | Fix role path          |
| Roles              | Role version mismatch         | App breaks                      | Git logs                                   | Unpinned role           | Pin role version       |
| Roles              | Role task override            | Unexpected behavior             | Play logs                                  | Vars conflict           | Fix vars               |
| **Handlers**       | Service not restarted         | Config change ignored           | Play output                                | Handler not notified    | Add notify             |
| Handlers           | Handler runs too often        | Unnecessary restarts            | Play output                                | Always notify           | Add condition          |
| **Idempotency**    | Tasks change every run        | Drift                           | Play output                                | Bad logic               | Fix idempotency        |
| Idempotency        | Shell task breaks idempotency | Repeated changes                | Play output                                | Using shell             | Use module             |
| **Permissions**    | Permission denied             | Task fails                      | Error logs                                 | Missing sudo            | Enable become          |
| Permissions        | File ownership wrong          | App can’t read config           | `ls -l`                                    | Wrong owner             | Fix ownership          |
| **Become (sudo)**  | Sudo password prompt          | Playbook stuck                  | Play output                                | Missing NOPASSWD        | Fix sudoers            |
| Become             | Wrong privilege escalation    | Task fails                      | Error logs                                 | Wrong become_user       | Fix become_user        |
| **Templates**      | Config wrong                  | App won’t start                 | Rendered file                              | Template error          | Fix Jinja              |
| Templates          | Variable not rendered         | App misconfig                   | File content                               | Missing var             | Fix vars               |
| **Loops**          | Loop fails                    | Task error                      | Play output                                | Wrong loop var          | Fix loop               |
| Loops              | Performance issue             | Slow task                       | Play logs                                  | Large loops             | Optimize logic         |
| **Conditionals**   | Task skipped wrongly          | App not deployed                | Play output                                | Bad `when`              | Fix condition          |
| **Tags**           | Wrong task executed           | Partial deploy                  | `--tags`                                   | Wrong tag usage         | Fix tags               |
| Tags               | Missing tasks                 | Deploy incomplete               | Play output                                | No tag                  | Add tags               |
| **Error Handling** | Play stops on error           | Partial deploy                  | Play output                                | No ignore               | Use `ignore_errors`    |
| Error Handling     | Hidden failures               | Silent issues                   | Logs                                       | `ignore_errors` misused | Handle errors properly |
| **Performance**    | Playbook very slow            | Long maintenance                | `-vvv`                                     | Too many serial tasks   | Parallelize            |
| Performance        | Network bottleneck            | SSH slow                        | SSH debug                                  | No control persist      | Enable SSH pipelining  |
| **CI/CD**          | Works locally not in CI       | Pipeline fails                  | CI logs                                    | Missing vars            | Add CI vars            |
| CI/CD              | Vault decrypt fails           | Pipeline red                    | Error output                               | Wrong vault key         | Fix vault password     |
| CI/CD              | Inventory missing             | CI deploy fails                 | Logs                                       | Wrong path              | Fix inventory path     |
| **Security**       | Plaintext passwords           | Audit issue                     | Code review                                | No vault                | Encrypt secrets        |
| Security           | Excess sudo access            | Security risk                   | Review sudoers                             | Over-privileged         | Limit sudo             |
| **Recovery**       | Partial deploy                | App down                        | Play logs                                  | Mid-play failure        | Re-run safely          |
| Recovery           | Need rollback                 | Bad deploy                      | Git history                                | No rollback plan        | Add rollback play      |


# GitLab CI/CD mega table

| Category                  | Production Problem          | Real Production Example     | Troubleshooting Commands / Checks             | Root Cause                      | Fix / Resolution (Exact Actions)           |
| ------------------------- | --------------------------- | --------------------------- | --------------------------------------------- | ------------------------------- | ------------------------------------------ |
| **Pipeline**              | Pipeline not triggered      | Code pushed but no pipeline | Check `.gitlab-ci.yml`, GitLab UI → Pipelines | `rules` / `only` misconfigured  | Fix rules (`rules: if: $CI_COMMIT_BRANCH`) |
| Pipeline                  | Pipeline stuck in pending   | Jobs never start            | GitLab UI → Runners                           | No runner available             | Register/start runner                      |
| Pipeline                  | Pipeline fails immediately  | YAML error                  | GitLab UI error                               | Invalid YAML                    | Fix `.gitlab-ci.yml` syntax                |
| Pipeline                  | Wrong pipeline runs         | Prod pipeline on dev branch | Review pipeline rules                         | Bad branch rules                | Fix `rules/only/except`                    |
| **Runners**               | Runner offline              | Jobs pending forever        | `gitlab-runner status`                        | Runner service down             | `systemctl restart gitlab-runner`          |
| Runners                   | Runner not picking jobs     | Jobs stuck                  | GitLab UI runner tags                         | Tag mismatch                    | Match job & runner tags                    |
| Runners                   | Runner permission denied    | Job fails                   | Job logs                                      | Wrong user perms                | Fix runner user permissions                |
| Runners                   | Runner disk full            | Jobs fail mid-build         | `df -h` on runner                             | Logs/artifacts filled disk      | Cleanup runner disk                        |
| **Checkout / Git**        | Repo clone fails            | Job fails at checkout       | Job logs                                      | Network / auth issue            | Fix repo access                            |
| Git                       | Submodule clone fails       | Build fails                 | Job logs                                      | Missing submodule creds         | Add SSH key/token                          |
| Git                       | Wrong branch checked out    | Deploy wrong code           | Job logs                                      | Wrong ref                       | Fix pipeline rules                         |
| **Variables / Secrets**   | Secret missing              | App deploy fails            | CI job logs                                   | Variable not set                | Add CI/CD variable                         |
| Variables                 | Secret exposed in logs      | Security incident           | CI logs                                       | Echoing secrets                 | Mask & protect variables                   |
| Variables                 | Wrong env value             | App misconfigured           | Deploy logs                                   | Env var mismatch                | Fix variable scope                         |
| Variables                 | Protected var not available | Prod deploy fails           | CI logs                                       | Running from unprotected branch | Protect branch                             |
| **Build**                 | Build fails randomly        | Flaky builds                | Retry logs                                    | Race condition                  | Add retries                                |
| Build                     | Build slow                  | Long pipeline               | Job timing                                    | No caching                      | Enable cache                               |
| Build                     | Cache not used              | Slow rebuild                | CI logs                                       | Wrong cache key                 | Fix cache key                              |
| Build                     | Dependency download fails   | Job timeout                 | Job logs                                      | Network issue                   | Use mirror / retry                         |
| **Docker (CI)**           | Docker build fails          | Image not built             | Job logs                                      | Dockerfile error                | Fix Dockerfile                             |
| Docker                    | Image pull fails            | Job fails                   | Job logs                                      | Registry auth issue             | `docker login`                             |
| Docker                    | Old image deployed          | Bug persists                | Image tag                                     | Reused `latest` tag             | Use versioned tags                         |
| Docker                    | Docker daemon not available | Job fails                   | Job logs                                      | DinD not enabled                | Enable `docker:dind`                       |
| **Artifacts**             | Artifact missing            | Deploy job fails            | Job logs                                      | Artifact expired                | Increase retention                         |
| Artifacts                 | Artifact too large          | Upload fails                | Job logs                                      | Size limit                      | Reduce artifact size                       |
| Artifacts                 | Wrong artifact used         | Old build deployed          | Job logs                                      | Artifact name mismatch          | Fix artifact paths                         |
| **Deployments**           | Deploy job fails            | App not deployed            | Deploy logs                                   | Wrong kube creds                | Fix kubeconfig                             |
| Deployments               | Deploy succeeds, app down   | Prod outage                 | App logs                                      | Bad config                      | Rollback                                   |
| Deployments               | Wrong env deployed          | Prod impacted               | Pipeline view                                 | Env rules wrong                 | Fix environment rules                      |
| Deployments               | Manual job skipped          | No deploy                   | Pipeline UI                                   | Job set manual                  | Trigger manually                           |
| **Environments**          | Env not updated             | Dashboard stale             | GitLab Envs                                   | Missing env name                | Add `environment:`                         |
| Environments              | Stop job not working        | Old pods running            | Env page                                      | Missing stop job                | Add stop action                            |
| **Kubernetes (CI/CD)**    | kubectl not found           | Deploy fails                | Job logs                                      | Missing CLI                     | Install kubectl                            |
| K8s                       | Auth error                  | Deploy fails                | Job logs                                      | Wrong kube token                | Update secret                              |
| K8s                       | Rollout stuck               | Pipeline hangs              | `kubectl rollout status`                      | Pod crash                       | Fix app                                    |
| K8s                       | Rollback needed             | Bad release                 | Job logs                                      | Bad image                       | `kubectl rollout undo`                     |
| **Security / Compliance** | Secrets leaked              | Audit alert                 | CI logs                                       | Plaintext secrets               | Use masked vars                            |
| Security                  | Unauthorized deploy         | Policy violation            | GitLab audit                                  | Bad permissions                 | Restrict roles                             |
| Security                  | Pipeline bypass             | Unreviewed deploy           | Merge history                                 | Missing approvals               | Enforce approvals                          |
| **Timeouts**              | Job timeout                 | Pipeline red                | Job logs                                      | Long-running task               | Increase timeout                           |
| Timeouts                  | Runner timeout              | Job killed                  | Runner logs                                   | Runner config                   | Increase runner timeout                    |
| **Performance**           | Pipeline too slow           | Long feedback loop          | Job duration                                  | Too many stages                 | Parallelize jobs                           |
| Performance               | Serial jobs                 | Slow deploy                 | Pipeline graph                                | No parallelism                  | Use `needs:`                               |
| **Failure Recovery**      | Bad deploy                  | Prod outage                 | Logs & history                                | Bad commit                      | Rollback                                   |
| Recovery                  | Partial pipeline failure    | Inconsistent state          | Pipeline logs                                 | No cleanup                      | Add cleanup job                            |
| Recovery                  | Need emergency fix          | Prod down                   | Git revert                                    | Bad commit                      | Revert commit                              |
| **Observability**         | No pipeline logs            | Debug impossible            | Runner logs                                   | Logging disabled                | Enable logs                                |
| Observability             | Missing metrics             | No visibility               | GitLab metrics                                | Monitoring off                  | Enable monitoring                          |


# SHELL SCRIPTING

This README explains **Shell Scripting** step‑by‑step with **clear examples**, covering:

- Comments  
- Printing  
- Variables  
- Functions  
- Conditions (if, nested if)  
- Loops (for, while, nested loops)  
- Inputs  
- Exit Status  
- Quotes  
- Redirection  
- sed  
- awk  

Designed for **Linux Admins, DevOps, SREs, and Interviews**.

---

## 1️⃣ What is Shell Scripting?
Shell scripting is writing a sequence of Linux commands in a file to automate tasks.

```bash
#!/bin/bash
echo "Hello Shell"
```

---

## 2️⃣ Comments
Comments explain what the code does.

```bash
# This is a comment
echo "Hello"
```

Used for:
- Documentation
- Debugging
- Maintenance

---

## 3️⃣ Printing (echo / printf)

```bash
echo "Hello World"
printf "User: %s\n" "$USER"
```

Use `printf` when formatting output is required.

---

## 4️⃣ Variables

```bash
name="Satish"
age=30
echo "Name: $name, Age: $age"
```

❌ Wrong:
```bash
name = satish
```

---

## 5️⃣ User Input

```bash
read -p "Enter your name: " name
echo "Hello $name"
```

---

## 6️⃣ Quotes

| Type | Purpose |
|----|----|
| Double quotes | Variable expansion |
| Single quotes | Literal text |
| Backticks / $( ) | Command substitution |

```bash
echo "$USER"
echo '$USER'
echo "Today is $(date)"
```

---

## 7️⃣ Exit Status

Every command returns a status:
- `0` → success
- Non‑zero → failure

```bash
ls /tmp
echo $?
```

Use in scripts:

```bash
ls /fake || echo "Command failed"
```

---

## 8️⃣ Conditions (if)

```bash
if [ $age -ge 18 ]; then
  echo "Adult"
else
  echo "Minor"
fi
```

### Nested if

```bash
if [ $age -ge 18 ]; then
  if [ $age -ge 60 ]; then
    echo "Senior"
  else
    echo "Adult"
  fi
else
  echo "Minor"
fi
```

---

## 9️⃣ Loops

### for loop

```bash
for i in 1 2 3; do
  echo "Number $i"
done
```

### while loop

```bash
count=1
while [ $count -le 3 ]; do
  echo "Count $count"
  ((count++))
done
```

### Nested loop

```bash
for i in 1 2; do
  for j in A B; do
    echo "$i $j"
  done
done
```

---

## 🔟 Functions

```bash
greet() {
  echo "Hello $1"
}

greet Satish
```

Why use functions?
- Reusability
- Cleaner code
- Modular scripts

---

## 1️⃣1️⃣ Redirection

| Symbol | Meaning |
|----|----|
| > | Overwrite |
| >> | Append |
| < | Input |
| 2> | Error |
| &> | All output |

```bash
ls > out.txt
ls wrong 2> err.txt
```

---

## 1️⃣2️⃣ sed (Stream Editor)

Used for **search & replace**.

```bash
sed 's/old/new/' file.txt
sed -i 's/8080/80/' app.conf
```

Real use:
- Update config files
- Mask sensitive data

---

## 1️⃣3️⃣ awk

Used for **column‑based text processing**.

```bash
awk '{print $1}' file.txt
awk -F: '{print $1}' /etc/passwd
```

Example:

```bash
df -h | awk '{print $1, $5}'
```

---

## 1️⃣4️⃣ Production Script Template

```bash
#!/bin/bash
set -euo pipefail

read -p "Enter environment: " ENV

deploy() {
  echo "Deploying to $ENV"
}

deploy
```

---
Happy scripting 🚀

































### Finding Files
**1. find**
- Search for files/directories in a given location.
Syntax:
```bash
find [path] [options] [expression]

find /path/to/search -name "filename" #Find by name
find /home/user -name "notes.txt" #Example
find /path/to/search -iname "filename" #Case-insensitive search

find /path/to/search -type f -name "*.log" #Find by file type
#(-type f = file, -type d = directory)

find /var/log -size +10M #Find by size (Find files larger than 10 MB)
```

**2. locate**
- Search files quickly using a prebuilt index/database.
- Find file by name
```bash
locate filename
```
- Update the database (if new files aren’t found)
```bash
sudo updatedb
```
⚡ locate is much faster than find but may not reflect recent changes unless you run updatedb.

### Wildcards in Linux
- Wildcards are special characters used in shell commands to match patterns in filenames or strings.

**1. * (Asterisk) → Matches zero or more characters**
```bash
ls *.txt
```
👉 Matches all files ending with .txt (e.g., a.txt, notes.txt, test123.txt).

**2. ? (Question Mark) → Matches exactly one character**
```bash
ls file?.txt
```
👉 Matches file1.txt, fileA.txt, but not file10.txt.

**3. [] (Square Brackets) → Matches any one character in the range/class**
```bash
ls file[1-3].txt
```
👉 Matches file1.txt, file2.txt, file3.txt.
```bash
ls file[a-c].txt
```
👉 Matches filea.txt, fileb.txt, filec.txt.

**4. {} (Brace Expansion) → Matches a sequence or set**
```bash
echo file{1..5}.txt
```
👉 Expands to: file1.txt file2.txt file3.txt file4.txt file5.txt
```bash
echo {apple,banana,orange}.txt
```
👉 Expands to: apple.txt banana.txt orange.txt

✅ Summary:
- * → zero or more characters
- ? → exactly one character
- [] → character set/range
- {} → explicit list or sequence

### Linux File Types
- In Linux, everything is treated as a file. You can see file types using:
```bash
ls -l
```
👉 The first character of the output indicates the file type.

**1. - Regular File**
```bash
-rw-r--r--  1 user user  1200 Sep  4  notes.txt
```
**2. d Directory**
```bash
drwxr-xr-x  2 user user  4096 Sep  4  Documents
```
**3. Symbolic Link (Soft Link)**
```bash
lrwxrwxrwx  1 user user    10 Sep  4  linkfile -> notes.txt 
```
**4. c Character Device File**
- Represents hardware devices that transfer data character by character.
- Examples: Keyboard, Mouse, Serial ports (/dev/tty, /dev/random).

**5. b Block Device File**
- Represents devices that handle data in blocks.
- Examples: Hard drives, USB drives (/dev/sda, /dev/sdb).

**6. s Socket File**
- Used for network communication between processes.
- Allows two processes to exchange data.
- Example: /var/run/docker.sock

**7. p Named Pipe (FIFO)**
- Special file for inter-process communication (IPC).
- Works in FIFO (First In, First Out) order.
- Example:
```bash
mkfifo mypipe
echo "hello" > mypipe
cat < mypipe
```
✅ Summary Table
| Symbol | File Type         | Example Use Case                     |
| ------ | ----------------- | ------------------------------------ |
| `-`    | Regular file      | Text, binary, scripts, configs       |
| `d`    | Directory         | Folders                              |
| `l`    | Symbolic link     | Shortcuts/pointers                   |
| `c`    | Char device       | Keyboard, mouse, serial ports        |
| `b`    | Block device      | Hard drives, USB drives              |
| `s`    | Socket            | Process communication (e.g., Docker) |
| `p`    | Named pipe (FIFO) | IPC within system processes          |


### Text Processing Commands
**1. sort**
- The sort command sorts lines of text files
```bash
sort --version
sort --help
sort file #Basic sorting (alphabetical order)
sort -r file #Reverse Order
sort -k2 file #Sort by a specific column (e.g., 2nd column)
```
**2. uniq**
- The uniq command filters out repeated/duplicate adjacent lines.
```bash
uniq file
uniq -c file #Count occurrences of each unique line
```
⚠️ Important: uniq only works on consecutive duplicate lines. That’s why we should always sort first before using uniq.

**3. Combining sort and uniq**
```bash
sort file | uniq #Remove duplicates from file
sort file | uniq -c #Count occurrences of unique lines
```

### File Comparison Commands
**1. diff**
- Compares two files line by line and shows the differences.
```bash
diff file1 file2
```

Example:
file1.txt
```bash
apple
banana
orange
```

file2.txt
```bash
apple
grapes
orange
```

Command:
```bash
diff file1.txt file2.txt
```

Output:
```yaml
2c2
< banana
---
> grapes
```
👉 Meaning: line 2 in file1 is banana, but in file2 it’s grapes.

**2. cmp**
- Compares two files byte by byte (faster, lower-level check).
```bash
cmp file1 file2
```

- If files are identical → no output.
- If files differ → shows first byte & line where difference occurs.

Example:
```bash
cmp file1.txt file2.txt
```

Output:
```bash
file1.txt file2.txt differ: byte 8, line 2
```

✅ When to use what?
- Use diff → when you want to see what the differences are (line-based).
- Use cmp → when you just want to know if two files are identical (byte-level).

### Word/Line/Byte Count – wc
- The wc command prints the number of lines, words, characters, and bytes in a file.
```bash
wc --version
wc --help
wc filename
```
👉 Output (3 columns by default):
```bash
lines   words   bytes   filename
```

✅ Summary:
- wc -l → line count
- wc -w → word count
- wc -c → byte count
- wc -m → character count
- wc Directory

### Compress & Un-compress Commands
**1. tar (Tape Archive)**
- Used to archive multiple files/directories into a single .tar file (without compression).
```bash
tar cvf file.tar . #Create an archive
tar xvf file.tar #Extract an archive
```
  - c → create
  - v → verbose (show files being archived)
  - f → filename
  - x → extract

**2. gzip (GNU Zip)**
- Used to compress files. Works on single files only.
```bash
gzip file.tar #Compress a file
gzip -d file.tar.gz # Un-Compress a file
# or
gunzip file.tar.gz
```
**3. Combining tar + gzip**
- Most common way to create compressed archives:
```bash
#Create compressed archive

tar cvf file.tar dir/      # make tar
gzip file.tar              # compress → file.tar.gz


# Extract compressed archive

gzip -d file.tar.gz        # uncompress → file.tar
tar xvf file.tar           # extract contents

```
✅ Shortcut: Use tar with -z for gzip in one step:
```bash
tar czvf file.tar.gz dir/  # Create tar.gz directly
tar xzvf file.tar.gz       # Extract tar.gz directly
```

### Truncate Command

- The truncate command is used to shrink or extend the size of a file to a specified size.
```bash
truncate -s SIZE filename
truncate -s 10 filename # Shrink file to 10 bytes
truncate -s 0 filename # Set file size to zero (empty the file)
