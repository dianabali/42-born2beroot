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
- Has a hostname of the form `<login>42`.
- Runs a `monitoring.sh` script every 10 minutes.
- Has a user `<login>` belonging to the `user42` and `sudo` groups.

### Operating system choice
**Debian** was chosen for this project because it is stable, well documented and easier to configure for a first system administration project.

## Main design choices
### Partitioning
- **/boot (sda1):** Kept outside the encrypted volume so the bootloader
  can load the kernel before the disk is unlocked.
- **Extended partition (sda2):** A 1K container that lets the disk hold
  more than four partitions. The logical partition sda5 lives inside it.
- **LUKS (sda5_crypt):** sda5 is encrypted with LUKS. The passphrase is
  asked at boot, and everything below it is protected at rest.
- **LVM:** The decrypted volume is a physical volume in the volume group
  `dbali42-vg`, split into logical volumes. LVM allows resizing volumes
  later without repartitioning.
  - `root` (7.5G): the system and installed software
  - `swap_1` (1G): swap space used when RAM is full
  - `home` (10.5G): user data, separate so that reinstalling or
    breaking the system doesn't affect personal files

Use ```lsblk``` to show the partitions.

### Security policies
- **Password policy** (`etc/login.defs` and `libpam-pwquality`):
	- Expires every 30 days, minimum 2 days between changes, warning 7 days before expiry.
	- Minimum 10 characters, at least one uppercase letter, one lowercase letter, one digit.
	- No more than 3 consecutive identical characters.
	- Must not contain the username.
	- Must differ from the previous password by at least 7 characters.
	- Applies to root as well.

- **sudo policy** (`etc/sudoers.d`):
	- Maximum 3 attempts on wrong password.
	- Custom error message on wrong password.
	- All sudo input and output logged in `/var/log/sudo/`.
	- TTY mode required.
	- Restricted `secure_path`.

- **SSH**: port4242, `PermitRootLogin no`.
- **Firewall**: UFW enables, default deny incoming, only port 4242 allowed.
- **Mandatory access control**: AppArmor enabled.

### User management
- Root account is not used for user management.
- A regular user `<login>` is a member of `sudo` and `user42`.
- Additional users and groups are created with `adduser` and `addgroup`.

### Services installed
- `openssh-server`
- `ufw`
- `sudo`
- `libpam-pwquality`
- `cron`
- `apparmor`

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
**Debian** is a free general-purpose operating system. It is the second oldest Linux distribution. It is used for servers, personal computers, and embedded devices. It is operated bt Software in the Public Interest. Handles security through public disclosure.

**Rocky Linux** is a free, open-source Linux distribution. Intended to be a complete release using the Red HAt Enterprise Linux (RHEL) OS source code. Aimed to provide a community-supported OS. 

### 2. AppArmor vs. SELinux
**AppArmor** is a Linux kernel security module that allows system administrators to restrict programs' capabilities with profiles. Supplements the traditional Unix discretionary access control (DAC) model by providing mandatory access control (MAC). Offered as an alternative to SELinux.

**SELinux** (Security-Enhanced Linux) is a Linux kernel security module that provides a mechanism for supporting access control, security policies, and MAC.

### 3. UFW vs. firewalld
**UFW** (Uncomplicated Firewall) is a program for managing netfilter firewall. Easy to use. Uses a command line interface and iptables for configuration. Available by default in all Ubuntu and Debian installations.

**firewalld** is a firewall management tool designed for Linux operating systems. Like UFW, it acts as a frontend for the Linux kernel's netfilter framework. Its current default backend is nftables. Is an alternative to nft and iptables command line programs.

### 4. VirtualBox vs. UTM
**VirtualBox** is a free, open-source program used to create and run virtual machines (VMs). It extends the capabilities of your computer so that it can run multiple OSs inside multiple VMs at the same time.

**UTM** is an open-source virtual machine application for macOS. Functions as a virtualizer (multiple OSs if the CPU architecture is the same) and emulator (simulating different hardware).

## Commands for Born2BeRoot:
Here lies my iconic [`COMMANDS.md`](https://github.com/dianabali/42-born2beroot/blob/main/COMMANDS.md) with every command I used during this project.

## Other useful commands:
**simple stuff:**

`sudo service ufw status` - check the status of UFW.

`sudo service ssh status` - check the status of SSH.

`uname -a` - check the OS.

`sudo hostnamectl` - see hostname and info.

**user stuff:**

`sudo adduser <login>` - add a new user.

`nano /etc/pam.d/common-passwd` - password policy.

`sudo groupadd <groupname>` - add a new group.

`cat /etc/group` - check all groups.

`sudo adduser <login> <group>` - add a user to a group.

`groups <login>` - check the groups of user.

`nano /etc/pam.d/common-password` - check password rules.

**hostname stuff:**

`sudo hostnamectl` - check the hostname of the machine.

`sudo hostnamectl set-hostname <user>` - change hostname.

`lsblk` - view partitions.

**sudo stuff:**

`sudo --version` - check if sudo is installed.

`ls -ld /var/log/sudo` - check that /var/log/sudo exists.

`sudo ls -la /var/log/sudo` - check the contents of /var/log/sudo.

`sudo journalctl -t sudo` - check sudo logs.

**ufw stuff:**

`sudo ufw --version` - check if UFW is installed.

`sudo service ufw status` - check that UFW is working.

`sudo ufw status` - check the active rules of UFW.

`sudo ufw allow 8080/tcp` - allow port 8080 to UFW.

`sudo ufw delete allow 8080/tcp` - delete the 8080 rule of UFW.

**ssh stuff:**

`sudo service ssh status` - check that SSH is active.

`sudo service ssh status` - check that SSh uses port 4242.

**script stuff:**

`nano /etc/profile` - check the weird scary code.

`sudo crontab -u root -e` - check the script timing with Cron.

`sudo nano /etc/profile` - make sure `monitoring.sh` is running when the machine starts.

`sudo nano /etc/profile` - comment out the path for the script to not run when the machine starts.

**other stuff:**

`nano /etc/login.defs` - password policies for days.

`cat /etc/sudoers.d/sudo_config` - sudo policy.

`cat /etc/group` - all groups.

`cat /etc/passwd` - print users.

`getent group developers` - check a group.

`sudo gpasswd -d username developers` - remove a user from a group.

`sudo groupdel developers` - delete a group.

`dpkg -l | grep openssh-server` - check that SSH is installed.

## Resources
1. [What is a Virtual Machine?](https://www.geeksforgeeks.org/operating-systems/virtual-machines-in-operating-system/)
3. [Debian](https://en.wikipedia.org/wiki/Debian)
4. [Rocky Linux](https://en.wikipedia.org/wiki/Rocky_Linux)
5. [AppArmor](https://en.wikipedia.org/wiki/AppArmor)
6. [SELinux](https://en.wikipedia.org/wiki/SELinux)
7. [UFW](https://en.wikipedia.org/wiki/Uncomplicated_Firewall)
8. [Firewalld](https://en.wikipedia.org/wiki/Firewalld)
9. [Oracle VirtualBox manual](https://www.virtualbox.org/manual/)
10. [UTM](https://en.wikipedia.org/wiki/UTM_(software))
11. [OpenSSH manual](https://man.openbsd.org/sshd_config)
12. [Born2BeRoot guide](https://noreply.gitbook.io/born2beroot)

### Use of AI
AI tools were used as a learning and productivity aid:
- for explaining concepts and tools.
- for structuring this README.
- for reviewing `monitoring.sh`.
- for understanding errors and commands.

AI output was verified against official documentation before being applied.
