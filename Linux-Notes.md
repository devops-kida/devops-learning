# Linux Booting Process

The Linux boot process happens in stages, each one handing off control to the next. Here's the full journey from pressing the power button to seeing your login screen.

1. **BIOS/UEFI (Firmware)**  
    When you power on your computer, the firmware (older systems: BIOS, newer: UEFI) runs first. It does a quick hardware check called POST (Power-On Self-Test), then looks for a bootable device (your hard drive/SSD).

2. **Bootloader (GRUB)**  
    The firmware hands control to the bootloader, usually GRUB (Grand Unified Bootloader) on Linux. GRUB's job:

  * Shows you a menu (if you have multiple OSes or kernel versions)
  * Loads the Linux kernel into memory
  * Loads the initramfs (a temporary mini filesystem with drivers needed early on)

3. **Kernel Initialization**  
  The Linux kernel itself starts running. It:

  * Initializes hardware (CPU, memory, devices) using drivers
  * Mounts the initramfs temporarily
  * Sets up core subsystems (memory management, process scheduling)
  * Eventually mounts your real root filesystem (/)
    
4. **Init System (systemd)**  
  The kernel starts the very first process — PID 1 — which on most modern distros is systemd. This is the "manager of everything":

  * Mounts remaining filesystems
  * Starts background services (networking, logging, etc.) in parallel where possible
  * Brings the system to a target "state" (like a graphical desktop or command-line multi-user mode)
    
5. **Login**
  Finally, you get a login prompt — either a graphical login screen or a terminal — and once you log in, your shell or desktop environment starts.


<img width="1472" height="840" alt="image" src="https://github.com/user-attachments/assets/81c5c5fd-f5cd-4ca0-860c-d45c4b69db21" />

**initramfs** exists because the kernel needs some drivers (like disk controllers) before it can even read your real filesystem — it's a temporary bootstrap environment.
