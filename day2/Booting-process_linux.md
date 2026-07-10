# Day 02 — Linux Boot Process

## What I learned today:

### Booting Process Overview
- The sequence starts the moment the system is powered on, and ends when the user logs in and gets the login prompt
- Includes: firmware initialization - loading the bootloader - loading the kernel - starting init system - launching system services - login

---

### Step 1: BIOS / UEFI
- Initializes hardware and performs POST

**BIOS (Basic Input Output System)**
- Saved on the motherboard

**UEFI (Unified Extensible Firmware Interface)**
- Modern replacement for BIOS
- Faster, more secure
- Uses GPT (GUID Partition Table) — allows more partitions than older schemes

### Step 2: POST (Power On Self Test)
- Checks hardware components
- If it fails, indicates the failure and stops the booting process

### Step 3: Bootloader
- Firmware locates a bootable device
- Once the boot device is selected, control is passed to the bootloader

**GRUB Bootloader**
- Loads the program that loads the OS kernel into memory and transfers control to it
- Loads the kernel and initramfs

### Step 4: initramfs (Initial RAM Filesystem)
- A temporary root filesystem loaded into RAM
- Contains all essential tools needed before the real root filesystem can be mounted

### Step 5: Kernel Initialization
- Kernel initializes hardware and mounts the temporary file system

### Step 6: System Initialization
- System starts and begins launching services
- Default init system used: **systemd** (units are config-managed by systemd)

---

### Firmware
- A form of microcode/program embedded in hardware devices to help them operate effectively

---

### GRUB (GRAND Unified Bootloader)
- Default bootloader for most Linux distributions
- Loads the kernel, initramfs, and provides a recovery mode

**GRUB2**
- Newer version of GRUB
- Saved in the **MBR** (Master Boot Record) of the boot disk, or on the **EFI System Partition**

**MBR (Master Boot Record)**
- First sector of the disk
- Supports up to 4 partitions
- Contains boot code and the partition table

---
**Takeaway:** Covered the complete Linux boot sequence today — from powering on the system through BIOS/UEFI, POST, bootloader (GRUB), kernel initialization with initramfs, and finally system services starting up via systemd. Also understood the difference between legacy BIOS/MBR and modern UEFI/GPT booting.
