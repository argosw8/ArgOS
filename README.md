# My Linux From Scratch Build

A personal ARM64 Linux system built from source using Linux From Scratch, with a custom kernel configuration and systemd setup. This repository preserves configuration files from the installed system.

## System

- Architecture: ARM64 / AArch64
- Kernel: installed as `7.1.8-lfs`
- Init system: systemd
- Bootloader: GRUB with UEFI
- Root filesystem: ext4
- EFI system partition: FAT32
- Networking: systemd-networkd with a DHCP configuration
- Build environment: a Debian ARM64 virtual machine running in QEMU on macOS

The exact Linux From Scratch book revision was not recorded in this repository.

## Repository contents

```text
kernel/
  config-7.1.8                 Saved Linux kernel build configuration
boot/
  grub.cfg                     GRUB boot configuration
etc/
  fstab                        Filesystem mount configuration
  os-release                   Operating system identification
  systemd/
    system/
      dbus.service             Local D-Bus service definition
      dbus.socket              Local D-Bus socket definition
    network/
      10-eth-dhcp.network       Network configuration
      99-default.link          Symbolic link to /dev/null
```

The D-Bus definitions are preserved as installed. Their presence here does not mean they are suitable for every systemd or D-Bus version.

## Scope

This is a configuration archive, not a bootable OS image or an automated build system. It does not contain the complete source tree, compiled kernel, installed packages, or virtual machine disks. The saved configurations alone are not enough to reproduce the entire system.

## Building a similar system

1. Follow the appropriate Linux From Scratch systemd instructions for your target architecture. This build targets ARM64; architecture-specific toolchain and boot instructions are required.
2. Build and install the required packages and Linux kernel. Use `kernel/config-7.1.8` as a reference for the kernel configuration, reviewing it against the kernel version and hardware you choose.
3. Install GRUB for AArch64 UEFI and configure the EFI system partition.
4. Review and adapt the saved configuration files before installing them into your new system.
5. Enable the services your system requires and verify boot, networking, and D-Bus operation.

Refer to the [Linux From Scratch project](https://www.linuxfromscratch.org/) for the build documentation. Detailed commands and exact package versions for this particular build have not yet been documented.

## Machine-specific settings

- Replace the filesystem UUIDs in `etc/fstab` with the UUIDs of your own root and EFI partitions. Obtain them with `blkid` or `lsblk -f`.
- Review `boot/grub.cfg` for disk identifiers, kernel paths, and boot arguments specific to the original virtual machine.
- Review the network configuration for the interface name and network environment of your machine.
- Preserve `etc/systemd/network/99-default.link` as a symbolic link to `/dev/null` if you intend to retain that setting.

## Excluded files

Virtual disks (`.qcow2`), installer ISOs, downloaded package archives, generated binaries, SSH keys, passwords, shell history, and runtime data are intentionally excluded.

## Project status

The system has been assembled in a QEMU environment. This repository currently documents selected installed configurations; a complete build guide and automated rebuild process remain future work.
