_This project has been created as part of the 42 curriculum by dbali._

# Born2Beroot

### Description
**Born2Beroot** is a system administration project from the 42 core curriculum. The goal of the project is to set up a minimal, hardened server inside a virtual machine, following the strict rules, and to understand why each choice matters: partitioning, user/group management, password policies, SSH, firewall, and sudo configuration/monitoring.

### Overview
The finished virtual machine:
- Runs `Debian` with no GUI.
- Uses `LVM` on top of an encrypted partition.
- Runs `UFW`.
- Exposes `SSH` only at port `4242`.
- Enforces a strong password policy and a strict sudo policy.
- Has a host name of the form `<login>42`.
- Runs a `monitoring.sh` script every 10 minutes.
- Has a user `<login>` belonging to the `user42` and `sudo` groups.

### Operating system choice
**Debian** was chosen for this project because it is stable, well documented and easier to configure for a first system administration project. It is community-driven distribution with a very large package repository, and it ships AppArmor enabled by default.

## Instructions
### Prerequisities
- VirtualBox
- Debian
- At least 30GB of virtual disk space.

### Configuration
1. Create a new VM with at least 1GB RAM and a 30GB disk.
2. Attach the `Debian ISO` and install in text mode.
3. Configure locals.
4. Set host name to `<login>42`.
5. Set up users and passwords.
6. Configure the clock.
7. Set up partition disks.
8. Configure the package manager.
9. Install the `GRUB` boot loader.
10. Boot into your VM.
11. Install `sudo` & configuration of user and groups.
12. Create a user/group and add the user to that group.
13. Install and configure `SSH`, and connect via SSH.
14. Install and configure `UFW` & `Firewall`.
15. Allow a port to firewall.
16. Configure `sudo` policies and password policies.
17. Create the script (`monitoring.sh`).
18. Configure `Crontab`.
19. Generate the signature (`signature.txt`).

**NOTE:** AFTER GENERATING THE SIGNATURE, **DO NOT** REOPEN THE VIRTUAL MACHINE, OTHERWISE THE SIGNATURE WILL CHANGE.

**ANOTHER NOTE:** DON'T FORGET TO CLONE THE VIRTUAL MACHINE.

## Comparisons
### 1. Debian vs. Rocky
**Debian** is a general-purpose Linux distribution for flexibility, simplicity, and a large software ecosystem.

**Rocky Linux** is the better fit for RHEL-compatible enterprise servers, especially when you need to mirror a Red Hat–style production environment.

| Criteria | Debian | Rocky Linux |
|---|---|---|
| Based on | Independent community distribution | RHEL-compatible distribution |
| Package format | `.deb` | `.rpm` |
| Package manager | `apt` / `dpkg` | `dnf` / `rpm` |
| Default security framework | AppArmor | SELinux |
| Default firewall tool | None enforced (UFW or nftables commonly used) | firewalld |
| Server use | Excellent | Excellent |
| Desktop use | Excellent | Possible, but more server-oriented |
| Release model | Stable releases (every 2 years) | Follows RHEL major releases |
| Target use | General purpose, servers, desktops | Enterprise servers |
| Learning curve | Gentler | More complex |

*Pick Debian:* If you're building a VPS, home server, web server, self-hosted services, development environment, or general Linux infrastructure without a specific enterprise compatibility requirement.

*Pick Rocky Linux:* If you're deploying enterprise applications, following RHEL-based standards, migrating from CentOS, or need a long-lived environment aligned with the enterprise Linux ecosystem.

### 2. AppArmor vs. SELinux
**AppArmor** is a Linux kernel security module that provides Mandatory Access Control (MAC). It lets system administrators restrict what individual programs can do by applying custom security profiles to them.

**SELinux** (Security-Enhanced Linux) is a security architecture built into the Linux kernel that implements Mandatory Access Control (MAC).

| Criteria | AppArmor | SELinux |
|---|---|---|
| Model | Path-based profiles | Label-based (security contexts on every file and process) |
| Complexity | Simple, easier to read and write profiles | Very powerful but complex |
| Granularity | Per-application confinement | Very fine-grained, system-wide policy |
| Default on | Debian, Ubuntu, SUSE | RHEL, Rocky, Fedora, CentOS |
| Strengths | Quick to deploy, low maintenance | Stronger isolation, suited to high-security environments |
| Weaknesses | Less granular, depends on file paths | Hard to debug, steep learning curve |

*Pick AppArmor:* You are using Ubuntu or Debian. AppArmor is built-in and enabled by default on these systems. Stick with it unless you have a strict requirement for label-based security.

*Pick SELinux:* You are using RHEL-family distributions. SELinux is natively integrated into Red Hat, Fedora, Rocky Linux, and AlmaLinux. Turning it off can break system components that rely on its security labels.

### 3. UFW vs. firewalld
**UFW** (Uncomplicated Firewall) is a user-friendly frontend tool for managing netfilter / iptables firewall rules in Linux.

**firewalld** is a dynamic firewall management tool designed for Linux operating systems. Like UFW, it acts as a frontend to manage the underlying kernel packet filtering frameworks (nftables or iptables).

| Criteria | UFW | firewalld |
|---|---|---|
| Full name | Uncomplicated Firewall | Dynamic firewall daemon |
| Backend | iptables / nftables | nftables (iptables legacy) |
| Concept | Simple rules (`allow`, `deny`) | Zones and services |
| Runtime changes | Reloads rules | Applies changes dynamically without dropping connections |
| Typical on | Debian, Ubuntu | RHEL, Rocky, Fedora |
| Best for | Simple setups and quick configuration | Complex environments with multiple network zones |

*Pick UFW:* If you are running Ubuntu or Debian: It is already installed, integrated into the system, and requires zero extra setup and want absolute simplicity.

*Pick firewalld:* You are running a RHEL-family OS. It is the native firewall daemon for Red Hat, Rocky, and Fedora. Or you have multiple network interfaces

### 4. VirtualBox vs. UTM
**VirtualBox** is a free, open-source hypervisor used to create and run virtual machines (VMs). It acts as a software emulator, allowing you to run multiple guest operating systems (like Linux, Windows, or macOS) simultaneously on a single physical computer (the host OS) without repartitioning your hard drive.

**UTM** is a popular, open-source virtualization and emulation software designed specifically for macOS.

| Criteria | VirtualBox | UTM |
|---|---|---|
| Type | Hypervisor (Type 2) | Frontend for QEMU / Apple Virtualization |
| Platforms | Windows, Linux, macOS (Intel) | macOS (Intel and Apple Silicon), iOS |
| Apple Silicon (ARM) | Limited support | Native support |
| Architecture emulation | x86 virtualization only | Virtualization and full emulation (x86 on ARM) |
| Performance | Good on x86 hosts | Near native when virtualizing, slower when emulating |
| License | Free (GPLv3 / extension pack) | Free (Apache 2.0), paid on the App Store |
| Interface | Mature and feature-rich | Simple and macOS-native |

*Pick VirtualBox:* If your main computer runs Windows or Linux, or if you have an older Intel-based Mac. It does not work reliably on newer Apple computers.

*Pick UTM:* Pick UTM if your main computer is a modern Apple Silicon Mac (M1, M2, M3, M4). UTM is built specifically for Apple's architecture and runs circles around VirtualBox on those machines.

## Resources
1. [What is a Virtual Machine?](https://www.geeksforgeeks.org/operating-systems/virtual-machines-in-operating-system/)
2. [What is UFW?](https://nhimg.org/glossary/ufw-firewall/)
3. [Debian vs. Rocky](https://amadla.medium.com/debian-linux-vs-rocky-os-exploring-the-best-choice-for-your-server-dfd6b3d80c1a)
4. [Debian](https://www.debian.org/doc/)
5. [Oracle VirtualBox manual](https://www.virtualbox.org/manual/)
6. [OpenSSH manual](https://man.openbsd.org/sshd_config)
7. [Born2BeRoot guide](https://noreply.gitbook.io/born2beroot)

### Use of AI
AI tools were used as a learning and productivity aid:
- for explaining concepts and tools.
- for structuring this README.
- for reviewing `monitoring.sh`.
- for understanding errors.

AI output was verified against official documentation before being applied.


## Commands for Born2BeRoot:
[My iconic `COMMANDS.md` with every command I used during this project](https://github.com/dianabali/42-born2beroot/blob/main/COMMANDS.md)

## Other useful commands:
`sudo ufw status` - check the status of UFW.

`sudo service ssh status` - check the status of SSH.

`uname -a` - check the OS.

`sudo hostnamectl` - see hostname and info.

`groups` - check the groups of user.

`cat /etc/group` - all groups.

`cat /etc/passwd` - print users.

`groups <user>` - check which groups a user belongs to.

`nano /etc/login.defs` - password policies for days.

`cat /etc/sudoers.d/sudo_config` - sudo policy.

`nano /etc/pam.d/common-passwd` - password policy.

`sudo hostnamectl set-hostname <user>` - change hostname.

`sudo --version` - check if sudo is installed.

`sudo adduser <login> <group>` - add a user to a group.

`ls -ld /var/log/sudo` - check that /var/log/sudo exists.

`sudo ls -la /var/log/sudo` - check the contents of /var/log/sudo.

`sudo journalctl -t sudo` - check sudo logs.

`sudo ufw --version` - check if UFW is installed.

`sudo ufw status` - check the active rules of UFW.

`sudo ufw allow 8080/tcp` - allow port 8080 to UFW.

`sudo ufw delete allow 8080/tcp` - delete the 8080 rule of UFW.

`dpkg -l | grep openssh-server` - check that SSH is installed.

`sudo service ssh status` - check that SSH is active.

`sudo service ssh status` - check that SSh uses port 4242.

`sudo crontab -u root -e` - check the script timing with Cron.

`sudo nano /etc/profile` - make sure `monitoring.sh` is running when the machine starts.



