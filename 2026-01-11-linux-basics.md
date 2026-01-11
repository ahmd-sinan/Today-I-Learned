# Linux & The Command Line 🐧

**Date:** 11-Jan-2026  

Today I learned about the Command Line Interface (CLI). Unlike a Graphical User Interface (GUI) where we click icons, the CLI allows us to execute text-based commands to interact directly with the Operating System.

## 1. The Terminal Environment 
The **Terminal** is the window where we type commands. The **Shell** is the program that interprets those commands.

| Feature | Linux | Windows |
| :--- | :--- | :--- |
| **Default Shell** | **Bash** (Bourne Again SHell) or **Zsh** | **Command Prompt** (cmd.exe) or **PowerShell** |
| **Path Separator** | Forward slash `/` (e.g., `/home/user`) | Backslash `\` (e.g., `C:\Users\User`) |
| **Case Sensitivity** | **Yes** (`File.txt` $\neq$ `file.txt`) | **No** (`File.txt` = `file.txt`) |
| **Root/Admin** | `sudo` (SuperUser Do) | "Run as Administrator" |

## 2. Basic Linux Commands 
These are the fundamental commands for navigating and managing files (File Management).

### Navigation & Creation
- **`ls`** → **List**
    - Lists all files and folders in the current directory.
- **`cd`** → **Change Directory**
    - Moves you into a different folder.
- **`mkdir`** → **Make Directory**
    - Creates a new folder.
    - *Usage:* `mkdir new_folder`

### Manipulating Files (Copy, Move, Remove)

#### **`cp`** → Copy
Used to duplicate files or directories.
- **Copy a file:**
    ```bash
    cp <source> <destination>
    cp hello.txt hai.txt
    # 'hai.txt' is now a duplicate of 'hello.txt'
    ```
- **Copy a directory (Recursive):**
    - We must use the `-r` (recursive) flag to copy a folder and everything inside it.
    ```bash
    cp -r <source_dir> <destination_dir>
    cp -r folder1 folder2
    # 'folder2' is now a duplicate of 'folder1'
    ```

#### **`mv`** → Move (or Rename)
Used to move a file to a new location OR rename it.
- **Rename a file:**
    ```bash
    mv <source> <destination>
    mv hello.txt hai.txt
    # 'hello.txt' is renamed to 'hai.txt' (or moved if destination is a folder)
    ```

#### **`rm`** → Remove (Delete) ⚠️
Used to delete files or directories. **Be careful:** There is no "Recycle Bin" in the terminal!

- **Remove a file:**
    ```bash
    rm <file>
    # Deletes the file. (Sometimes asks for confirmation: yes/no?)
    ```
- **Force remove a file:**
    ```bash
    rm -f <file>
    # -f = force. Deletes immediately without asking.
    ```
- **Remove a directory:**
    ```bash
    rm -r <dir>
    # -r = recursive. Deletes folder and contents. (Might ask for confirmation)
    ```
- **Force remove a directory (The "Nuclear" Option):**
    ```bash
    rm -rf <dir>
    # -rf = recursive + force.
    # Deletes the folder and everything inside it immediately without asking.
    # USE WITH EXTREME CAUTION!
    ```

---
**Key Takeaway:** The CLI is faster and more powerful than using a mouse, but it requires precision—especially when using commands like `rm -rf`!