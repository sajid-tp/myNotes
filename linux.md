# Linux

## 1. Definition

**Linux** is an open-source operating system kernel that manages computer hardware resources and provides essential services for applications to run.

It acts as an intermediary between applications and computer hardware.

### Important Distinction

- **Linux** is technically a **kernel**.
- **Ubuntu**, **Debian**, and **Fedora** are Linux-based operating system distributions.
- The kernel manages the CPU, memory, processes, devices, storage, and networking.

## 2. Why Do We Use Linux?

Applications need computer resources to function.

For example, a Node.js application needs:

- **CPU** to execute JavaScript.
- **RAM** to store variables and data.
- **Storage** to read and write files.
- **Network access** to communicate with clients.

If every application had to manage hardware directly, development would become extremely complicated.

Linux solves this problem by managing hardware resources and providing a standardized interface through which applications can access them.

It also provides an environment to run backend applications, web servers, and databases.

## 3. How Does Linux Work?

Linux works as the central layer between applications and hardware.

It can be understood through three layers:

### A. Hardware Layer

- CPU
- RAM
- SSD/HDD
- Network interfaces
- Other physical devices

### B. Kernel Layer

The Linux kernel manages these resources:

- **Process management:** Creates, schedules, and terminates processes.
- **Memory management:** Allocates and manages RAM.
- **CPU management:** Schedules processes and threads for CPU execution.
- **File management:** Provides interfaces to access files and storage.
- **Network management:** Handles network communication.

### C. Application Layer

Applications run on top of the operating system.

Examples:

- Node.js
- Nginx
- MongoDB
- SSH

Applications request operating system services through **system calls**. The kernel performs the necessary operations and returns the results.

## 4. Advantages

- **Open-source:** Its source code is publicly available.
- **Stability:** Suitable for long-running server environments.
- **Security:** Provides user permissions, access control, and security mechanisms.
- **Performance:** Can operate efficiently with limited resources.
- **Flexibility:** Can be customized according to requirements.
- **Developer-friendly:** Supports popular backend technologies and hosting tools.

## 5. Disadvantages

- **Learning curve:** Requires familiarity with terminal commands and system administration.
- **Software compatibility:** Some Windows-specific applications are not natively supported.
- **Hardware compatibility:** Some devices may have limited driver support.
- **Administration:** Requires knowledge of permissions, networking, processes, and configuration.

## 6. Example

### Scenario: Hosting a MERN Application on AWS EC2

Suppose we have developed an e-commerce application using **React**, **Node.js**, **Express**, and **MongoDB**.

We want to make it accessible to users through the internet.

We create an **EC2 instance** running **Ubuntu Linux**.

Linux provides the environment required to:

- Install Node.js and Nginx.
- Run our Express application.
- Manage the application’s processes.
- Allocate CPU and memory.
- Manage application files and directories.
- Handle network communication.
- Control user permissions.

For example, when we execute:

```bash
node server.js
```

Linux creates and manages the process that runs our Node.js application.

### Real-World Understanding

Node.js executes our application code, while Linux manages the underlying computer resources required to run it.
