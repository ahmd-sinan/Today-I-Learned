# The Linux Boot Process: From Hardware to GUI 🚀

**Date:** 2026-05-29
**Category:** Operating Systems / Linux
**Tags:** #Linux #BootProcess #Kernel #GRUB #Systemd

Today I mapped out the exact chronological sequence of events that occurs the moment a Linux machine is powered on. It happens in 7 distinct stages!

## The 7 Stages of Booting

1. **BIOS / UEFI:** First, it initializes the hardware and locates the bootloader on your storage drive.
2. **Bootloader (GRUB):** It displays the boot menu and loads both the Linux Kernel and initramfs into memory.
3. **initramfs (Initial RAM Filesystem):** This acts as a temporary mini-filesystem containing essential drivers the kernel needs to access your actual hard drive.
4. **Kernel:** The kernel takes control, mounts the real root filesystem, and prepares to start the very first user application.
5. **`/sbin/init`:** This is the absolute first user-space program executed by the kernel, forever holding Process ID (PID) 1.
6. **Init System (systemd or SysVinit):** This is the actual program acting as `/sbin/init`. Modern systems use systemd, while older ones use SysVinit to boot up all system services.
7. **Targets / Runlevels:** The init system fires up network services, background processes, and finally launches your GUI or TTYs.
8. 
