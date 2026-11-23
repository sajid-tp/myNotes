# 7. PWD (Print Working Directory)

## 1. Definition

`pwd` stands for **Print Working Directory**. It is a Linux command used to display the absolute path of the current working directory.

The **current working directory** is the directory in which the shell is currently operating.

> **In simple terms:** The `pwd` command tells us our current location in the Linux filesystem.

## 2. Why Do We Use PWD?

Linux contains a hierarchical filesystem with many directories and subdirectories.

When working on a remote server, we frequently navigate between different directories using the `cd` command.

Sometimes, we may forget our current location.

For example, imagine we are working with a MERN application containing:

- `backend`
- `frontend`
- `config`
- `logs`
- `uploads`

If we execute commands without knowing our current location, we might accidentally create, modify, or delete files in the wrong directory.

`pwd` solves this problem by displaying our current working directory as an absolute path.

It helps us:

- Identify our current location
- Verify directory navigation
- Avoid mistakes when executing file operations
- Understand where relative paths will be resolved

## 3. How Does PWD Work?

The `pwd` command displays the current working directory maintained by the shell or obtained from the operating system.

When we execute:

```bash
pwd
```

The following happens:

1. The shell receives the command.
2. It obtains the current working directory.
3. It displays the absolute directory path.
4. We can use this information to understand our current location.

### Syntax

```bash
pwd [options]
```

### Common Options

| Command | Purpose |
|---|---|
| `pwd` | Displays the current working directory |
| `pwd -L` | Displays the logical path, preserving symbolic links where possible |
| `pwd -P` | Displays the physical path, resolving symbolic links |

### Important Concepts

- **Absolute path:** Complete path starting from the root directory `/`
- **Current working directory:** The directory in which the shell is currently operating
- **Symbolic link:** A special filesystem entry that points to another file or directory

## 4. Advantages

- **Easy identification:** Shows our current location in the filesystem.
- **Prevents mistakes:** Helps avoid performing operations in the wrong directory.
- **Absolute path:** Displays the complete directory path.
- **Better navigation:** Works alongside `cd` and `ls`.
- **Useful for debugging:** Helps identify directory-related problems when executing commands.

## 5. Disadvantages

- **Limited functionality:** Only displays the current directory; it does not navigate between directories.
- **No file listing:** Does not display the contents of a directory.
- **No directory creation:** Cannot create or modify directories.
- **Limited information:** Does not explain the contents or structure of the current directory.

## 6. Example

### Scenario: Working With a MERN Application on an AWS EC2 Ubuntu Server

Suppose our project has the following structure:

```text
/home/ubuntu/myproject/
├── backend/
│   ├── server.js
│   └── package.json
└── frontend/
    ├── src/
    └── package.json
```

### Step 1: Navigate to the Project Directory

```bash
cd /home/ubuntu/myproject
```

### Step 2: Check Our Current Location

```bash
pwd
```

Output:

```text
/home/ubuntu/myproject
```

### Step 3: Navigate to the Backend Directory

```bash
cd backend
```

### Step 4: Check Our Location Again

```bash
pwd
```

Output:

```text
/home/ubuntu/myproject/backend
```

### Step 5: Move to the Parent Directory

```bash
cd ..
```

### Step 6: Verify the Location

```bash
pwd
```

Output:

```text
/home/ubuntu/myproject
```

## Remember

- `pwd`: Displays the current working directory.
- `cd`: Changes the current working directory.
- `ls`: Lists files and directories.
- `/`: Represents the root directory.
- **Absolute path:** Complete path starting from `/`.
- **Relative path:** Path specified based on the current working directory.
