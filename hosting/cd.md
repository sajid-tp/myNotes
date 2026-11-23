# 6. CD (Change Directory)

## 1. Definition

`cd` stands for **Change Directory**. It is a Linux shell command used to navigate between directories in the filesystem by changing the current working directory.

The **current working directory** is the directory in which the shell is currently operating.

> **In simple terms:** The `cd` command allows us to move from one folder to another in Linux.

## 2. Why Do We Use CD?

When working with a Linux server, files and directories are organized into a hierarchical filesystem.

For example, a MERN project might have the following structure:

```text
myproject
├── backend
└── frontend
```

To create, access, edit, or execute files inside these directories, we need to navigate to the appropriate location.

Without `cd`, we would frequently need to specify the complete file path when executing commands.

`cd` solves this problem by allowing us to navigate between directories easily through the terminal.

## 3. How Does CD Work?

The `cd` command changes the shell’s current working directory.

When we execute:

```bash
cd myproject
```

The shell:

1. Receives the directory path.
2. Resolves the path relative to the current directory or from the filesystem root.
3. Checks whether the directory exists and whether the user has permission to access it.
4. Changes the shell’s current working directory.
5. Maintains this working-directory information, so subsequent relative paths are interpreted from that location.

### Syntax

```bash
cd [directory_path]
```

Here:

- `cd`: Command used to change directories
- `directory_path`: The destination directory

## Important Navigation Concepts

### A. Absolute Path

An absolute path specifies the complete location of a directory starting from the root directory `/`.

Example:

```bash
cd /home/ubuntu/myproject
```

### B. Relative Path

A relative path specifies the destination based on the current working directory.

Example:

```bash
cd myproject
```

### C. Special Directory Symbols

| Symbol | Meaning |
|---|---|
| `.` | Current directory |
| `..` | Parent directory |
| `/` | Root directory |
| `~` | Current user’s home directory |
| `-` | Previous working directory |

## Common Commands

| Command | Purpose |
|---|---|
| `cd myproject` | Enters the `myproject` directory |
| `cd ..` | Moves one directory up |
| `cd /` | Moves to the root directory |
| `cd ~` | Moves to the user’s home directory |
| `cd -` | Returns to the previous working directory |
| `cd ../folder` | Moves to a sibling directory |
| `cd /home/ubuntu` | Navigates using an absolute path |

## 4. Advantages

- **Easy navigation:** Allows movement between directories through the terminal.
- **Saves time:** Avoids repeatedly specifying complete file paths.
- **Supports relative paths:** Makes navigation easier within project directories.
- **Supports absolute paths:** Allows direct navigation to any accessible directory.
- **Improves productivity:** Makes server administration and file management easier.

## 5. Disadvantages

- **Directory dependency:** Relative paths depend on the current working directory.
- **Path errors:** Incorrect directory names or paths result in errors.
- **Permission restrictions:** Users cannot enter directories they do not have permission to access.
- **No file navigation:** `cd` works with directories, not individual files.

## 6. Example

### Scenario: Navigating Through a MERN Application Directory on an AWS EC2 Ubuntu Server

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

### Step 1: Check the Current Location

```bash
pwd
```

This displays the current working directory.

### Step 2: Navigate to the Project Directory

```bash
cd /home/ubuntu/myproject
```

### Step 3: Navigate to the Backend Directory

```bash
cd backend
```

### Step 4: Check the Current Location

```bash
pwd
```

Output:

```text
/home/ubuntu/myproject/backend
```

### Step 5: Move One Directory Up

```bash
cd ..
```

This returns to:

```text
/home/ubuntu/myproject
```

### Step 6: Navigate to the Frontend Directory

```bash
cd frontend
```

This moves into the `frontend` directory.

## Remember

- `cd`: Changes the current working directory.
- `pwd`: Displays the current working directory.
- `ls`: Lists files and directories.
- `.`: Represents the current directory.
- `..`: Represents the parent directory.
- `/`: Represents the root directory.
- `~`: Represents the current user’s home directory.
- **Absolute path:** Complete path starting from `/`.
- **Relative path:** Path specified relative to the current directory.
