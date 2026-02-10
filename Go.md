# GO

***What is Go?***
- Go is a cross-platform, open source programming language
- Go can be used to create high-performance applications
- Go is a fast, statically typed, compiled language known for its simplicity and efficiency
- Go was developed at Google by Robert Griesemer, Rob Pike, and Ken Thompson in 2007
- Go's syntax is similar to C++

***What is Go Used For? (Explained Simply)***
- Web development (server-side)
  - Go is great for building fast and scalable web servers and APIs. Many modern web apps and backend services use Go because it handles high traffic efficiently.
- Developing network-based programs
  - Go has built-in support for networking, making it ideal for building tools like proxies, load balancers, and networking utilities.
- Cross-platform enterprise applications
  - Because Go is cross-platform, you can build applications that run on Windows, Linux, or macOS without changing the code. This makes it useful for large enterprise systems.
- Cloud-native development
  - Go is widely used in cloud technologies. Many cloud tools like Docker, Kubernetes, and Terraform are written in Go because it is fast, reliable, and handles concurrency very well.

***Go vs Python vs C++ (Comparison Table)***
| Feature                | **Go**                           | **Python**                                           | **C++**                              |
| ---------------------- | -------------------------------- | ---------------------------------------------------- | ------------------------------------ |
| **Typing**             | Statically typed                 | Dynamically typed                                    | Statically typed                     |
| **Runtime Speed**      | Fast                             | Slow                                                 | Fast                                 |
| **Execution**          | Compiled                         | Interpreted                                          | Compiled                             |
| **Compile Time**       | Fast compile time                | Interpreted (no real compile step)                   | Slow compile time                    |
| **Concurrency**        | Built-in (goroutines & channels) | No built-in concurrency (uses threads via libraries) | Supports concurrency through threads |
| **Garbage Collection** | Yes                              | Yes                                                  | No                                   |
| **OOP Support**        | No classes & objects             | Has classes & objects                                | Has classes & objects                |
| **Inheritance**        | No                               | Yes                                                  | Yes                                  |

***Imp Points:***
- Statically typed means that when you write a program, the data type of each variable must be known before the program runs. This is checked during compilation.
  In simple terms:
  - You must tell the computer what type of data (number, text, boolean, etc.) a variable will store before the program starts running.
  ```go
  var age int = 25
  ```
  - Here, age is declared as an int, and it cannot change to another type later.

- Dynamically typed means the variable’s data type is decided while the program is running, not before. You don’t need to declare the type. The language figures it out automatically.
  ```python
  x = 10      # x is an integer
  x = "hello" # now x becomes a string
  ```
  - The variable x can change its type anytime.
 
***Compiled vs Interpreted Languages (Easy Explanation)***
- Compiled Language
  - The entire program is translated into machine code before running.
  - This machine code is saved as an executable file (.exe, binary, etc.).
  - Runs very fast.
  - Examples: Go, C, C++, Rust
  - Simple meaning:
  ```Code → Compiler → Machine code → Run```

- Interpreted Language
  - Code is read and executed line by line at runtime.
  - No separate executable file is created.
  - Slower than compiled languages.
  - Examples: Python, JavaScript, PHP
  - Simple meaning:
    ```Code → Interpreter → Run (line by line)```

- Simple Definition of Concurrency
  - Concurrency means doing multiple tasks at the same time (or switching between them quickly) without affecting the final result.
  - Concurrency = handling many tasks together efficiently.
  - Example: A web server can handle hundreds of users at once — that’s concurrency.
  - Example 1: Web Server
    - A web server can handle many users at the same time:
      - User A requests a page
      - User B logs in
      - User C downloads a file
    - All these happen concurrently, even if the server has only one CPU. It quickly switches between tasks so users don’t feel any delay.
  - Example 3: Go Program (Conceptual)
    - In Go, you can run two tasks at the same time:
    ```go
    go task1()
    go task2()
    ```
  - Both tasks run concurrently using goroutines.
  - All these happen concurrently, even if the server has only one CPU.
  - It quickly switches between tasks so users don’t feel any delay.

  | Concept         | Meaning                                                        |
  | --------------- | -------------------------------------------------------------- |
  | **Concurrency** | Many tasks handled together (not necessarily at the same time) |
  | **Parallelism** | Many tasks executed **at the same time**                       |

- Notes:
  - Compilation time refers to translating the code into an executable program
  - Concurrency is performing multiple things out-of-order, or at the same time, without affecting the final outcome
  - Statically typed means that the variable types are known at compile time

***Why do we use go mod init?***
- Starts a new Go project
- Creates a go.mod file to manage the project
- Helps Go track dependencies (external packages)
- Allows you to use imports from outside
- No need for GOPATH — project can be anywhere
- Required for go run, go build, go get, etc.
- Super Simple:
  - go mod init = Tell Go “this is my new project, manage everything for me.”
