# Linux Services: Mastering `systemctl` 

**Date:** 2026-05-30

Today I learned how to control the background processes of a Linux system using `systemctl`. This is exactly how servers keep websites and databases running 24/7!

## 1. What is `systemd`?
* `systemd` manages all background services.
* We use the `systemctl` command for a specific service running in the background.
* These commands are used for background services only, like a Bluetooth manager, web server like apache, firewall, database, etc..

## 2. The Core `systemctl` Commands 
*(Note: These commands usually require `sudo` privileges because changing system services affects the whole OS!)*

Here are the commands using the `apache2` web server as an example:

*   **Start:** `sudo systemctl start apache2`
*   **Stop:** `sudo systemctl stop apache2`
*   **Restart:** `sudo systemctl restart apache2`
*   **Enable:** `sudo systemctl enable apache2`
*   **Disable:** `sudo systemctl disable apache2`
*   **Status:** `sudo systemctl status apache2` (To see enables or disables)

---

## Deep Dive: Start vs. Enable
While taking these notes, I realized it is incredibly important to understand the difference between these commands:

*   **`start` / `stop`:** This turns the service on or off *right now* for your current session. If you restart the computer, the service will go back to its default state.
*   **`enable` / `disable`:** This modifies the Linux boot process! Using `enable` tells the system, "Every time I turn this computer on, I want you to start this service automatically." It does *not* necessarily start the service right this second; it just queues it up for the next boot!