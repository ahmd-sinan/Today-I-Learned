# Linux Filesystem Hierarchy Standard (FHS) & Directory Structure 

**Date:** 2026-06-26
**Category:** Linux / System Administration
**Tags:** #Linux #FHS #SysAdmin #Filesystem #Architecture

Today I studied the complete structural blueprint of the Linux Operating System. To effectively manage and troubleshoot a Linux server, a developer must understand the standardized rules that dictate exactly where software, hardware files, and data reside.

## 1. What is the FHS? (Filesystem Hierarchy Standard) 📐
The **Filesystem Hierarchy Standard (FHS)** is the universal, industry-wide specification that defines the directory structure in Linux distributions. 

* **The Big Benefit (Predictability):** Because of the FHS, if you learn how to locate configuration files on an Ubuntu system, you will automatically know where they are on Fedora, Debian, or Arch.
* **Current Specification:** FHS 3.0 (maintained by FreeDesktop.org).

## 2. The Master Root Directory Cheat Sheet 
Here is the complete breakdown of the top-level directories under the Root (`/`), including what they do, real-world examples, and their Windows equivalents.

| Directory | What it is (Technical Function) | Industry Examples | Windows Equivalent 🪟 |
| :--- | :--- | :--- | :--- |
| **`/` (Root)** | The absolute top of the file tree. Every other file and folder is nested inside here. | N/A | `C:\` drive |
| **`/home`** | Personal directories for standard users to store documents, downloads, and user-specific configs. | `/home/student/` | `C:\Users\` |
| **`/root`** | The strictly private, isolated home folder specifically for the *Superuser* (Admin). | `/root/.ssh/` | *None* |
| **`/etc`** | The central hub for system-wide configuration and settings files. | `/etc/fstab`, `/etc/ssh/sshd_config` | Registry / `System32` |
| **`/bin`** | Essential, everyday binary executables (commands) available to all users. | `ls`, `cp`, `mkdir` | `System32` |
| **`/sbin`** | System binaries (admin tools) generally requiring root/sudo privileges to execute. | `fdisk`, `iptables` | `System32` |
| **`/lib`** | Shared library files (`.so` files) required by the programs running in `bin` and `sbin`. | `libc.so.6` | `.dll` files |
| **`/var`** | Variable data that changes constantly during system operation (logs, caches, web server roots). | `/var/log/syslog`, `/var/www/html` | Spread across `C:` |
| **`/tmp`** | Temporary file storage used by applications. **Warning:** This directory is usually wiped clean on every reboot! | Session caches | `C:\Windows\Temp\` |
| **`/dev`** | Device files. In Linux, physical hardware components are interacted with as if they were files. | `/dev/sda1` (Hard drive) | Device Manager |
| **`/proc`** | A virtual filesystem containing real-time data about running processes and kernel status. | `/proc/cpuinfo` | *None* |
| **`/sys`** | A modern virtual filesystem linking to hardware device data and drivers. | `/sys/class/net/` | *None* |
| **`/boot`** | The crucial, static files required by the Boot Loader to start the operating system (Kernel, initramfs). | `/boot/grub/grub.cfg` | Boot Manager |
| **`/run`** | Ephemeral runtime data (Process IDs, lock files) generated since the system was last booted. | `/run/systemd/` | *None* |

## 3. The Core Architectural Split: `/` vs `/usr` 
Historically, Unix systems physically separated the bare essentials from general software due to the storage limitations of early hard drives.

* **The Root Level (`/`): The Emergency Kit **
    Directories immediately under the root (like `/bin` or `/sbin`) traditionally contained only the absolute minimum utilities required to boot the machine or execute basic emergency repairs.
* **The `/usr` Level: The Main Software Hub 💻**
    Standing for "Unix System Resources," this contains the massive collection of general programs, libraries, and tools that make up the fully functioning, daily operating system. 

### The Modern Plot Twist (The "usrMerge") 
Because modern storage is massive, physically separating minimal boot tools from the rest of the OS is obsolete. Today, major distributions (Fedora, Arch, Ubuntu) use the **usrMerge**. On a modern system, the root directories (`/bin`, `/sbin`, `/lib`) are actually just **symbolic links (shortcuts)** pointing directly into their `/usr` counterparts (`/usr/bin`, `/usr/sbin`). They are logically the exact same place!

## 4. The `/usr` Hierarchy 
When installing general software, it routes into the `/usr` structure:
* **`/usr/bin`:** The main hub for the vast majority of everyday user programs (e.g., `python3`, `git`, `gcc`).
* **`/usr/sbin`:** Non-essential system admin tools (e.g., `useradd`, `sshd`).
* **`/usr/share`:** Architecture-independent data (e.g., `man` pages, icons, dictionaries).
* **`/usr/local`:** The DIY Directory! If you manually compile software from source code instead of using a package manager like `apt`, it installs here (specifically `/usr/local/bin`) so it doesn't conflict with official system updates.

## 5. Removable Media (Mounting under FHS) 
Because "Everything is a file or a directory" in Linux, plugging in external media does not generate a new drive letter (`E:` or `F:`). The OS logically attaches it to the existing tree.

* **Traditional Mounts:** System admins manually mount static drives to the `/mnt` directory.
* **Automated Mounts:** Modern Desktop Environments automatically mount USBs dynamically to: 
    `/run/media/[username]/[disk_label]`
    * *Example:* If logged in as `student` and you plug in a USB named `BACKUP`, the OS automatically exposes all those files inside the `/run/media/student/BACKUP/` folder!