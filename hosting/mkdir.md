# 4. MKDIR (Make Directory)

## 1. Definition

`mkdir` stands for **Make Directory**. It is a Linux command-line utility used to create new directories (folders) in the filesystem.

It allows users to organize files and applications by creating directory structures through the terminal.

> **In simple terms:** The `mkdir` command is used to create new folders in Linux.

## 2. Why Do We Use MKDIR?

While working with Linux, we need to organize files and applications into different directories.

For example, when deploying a MERN application, we might need separate directories for:

- Backend files
- Frontend files
- Configuration files
- Logs
- Backups

Without a way to create directories through the terminal, managing server files would become inconvenient.

`mkdir` solves this problem by allowing us to create directories directly through the command line.

## 3. How Does MKDIR Work?

The `mkdir` command interacts with the Linux filesystem to create new directory entries.

When we execute:

```bash
mkdir myproject
```

The following happens:

1. Linux receives the command through the shell.
2. The operating system checks the requested directory path.
3. It checks whether the user has permission to create a directory there.
4. The filesystem creates the new directory.
5. The new directory becomes available for storing files and other directories.

### Syntax

```bash
mkdir [options] directory_name
```

Here:

- `mkdir`: Command used to create directories
- `options`: Additional instructions that control directory creation
- `directory_name`: Name of the directory to create

### Common Commands

| Command | Purpose |
|---|---|
| `mkdir myproject` | Creates a directory |
| `mkdir folder1 folder2` | Creates multiple directories |
| `mkdir -p parent/child` | Creates parent and child directories, including missing parent directories |
| `mkdir -v myproject` | Displays a message when creating a directory |

## 4. Advantages

- **Easy organization:** Creates directories to organize files.
- **Quick execution:** Creates directories directly through the terminal.
- **Multiple directories:** Allows creation of multiple directories in one command.
- **Nested directories:** The `-p` option creates parent and child directories together.
- **Automation:** Can be used in shell scripts to create directory structures automatically.

## 5. Disadvantages

- **Existing directories:** Creating a directory with an existing name generally results in an error.
- **Permission restrictions:** Requires appropriate permissions to create directories in the specified location.
- **No automatic cleanup:** Creating directories does not manage or remove unnecessary files.
- **Path errors:** Incorrect paths can result in directories being created in unintended locations.

## 6. Example

### Scenario: Creating a Project Directory on an AWS EC2 Ubuntu Server

Suppose we want to organize our MERN application files on the server.

### Step 1: Create the Main Project Directory

```bash
mkdir myproject
```

This creates a directory named `myproject`.

### Step 2: Enter the Directory

```bash
cd myproject
```

This moves into the newly created directory.

### Step 3: Create Backend and Frontend Directories

```bash
mkdir backend frontend
```

This creates two directories inside `myproject`.

### Step 4: Create Nested Directories

```bash
mkdir -p backend/config
```

This creates the `config` directory inside `backend`. If the parent directory does not exist, it creates that too.

### Resulting Directory Structure

```text
myproject/
├── backend/
│   └── config/
└── frontend/
```

## Remember

- `mkdir`: Creates directories.
- `mkdir -p`: Creates parent and nested directories when necessary.
- `mkdir -v`: Displays information about created directories.
- `cd`: Changes the current working directory.
- `pwd`: Displays the current working directory.
