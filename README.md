## How to install `sudo`
1. Go to root user:
```bash
su
```
2. Install sudo package:
```bash
apt install sudo
```
3. Reboot the system:
```bash
sudo reboot
```
4. Verify if sudo was installed:
```bash
su
sudo -V
```

## How to create a user
```bash
sudo adduser <username/login>
```
```bash
sudo adduser dbali
```

## How to create a group
```bash
sudo addgroup <group-name>
getent group <group-name>  # verify
```
```bash
sudo addgroup user42
```

## How to add a user to a group
```bash
sudo adduser <username/login> <group-name>
getent group <group-name>  # verify
```
```bash
sudo adduser dbali user42
sudo adduser dbali sudo
getent group sudo user42
```

## How to install/configure SSH
### Install
1. Update the system
```bash
sudo apt update
```
2. Install OpenSSH Server:
```bash
sudo apt install openssh-server
sudo service ssh status  # verify
```

### Configure (with Nano)
1. Edit the SSH configuration file:
```bash
su  # switch to root
nano /etc/ssh/sshd_config  # Modify it
```
2. Change port 22 to port 4242
3. Disable root login via SSH:
- Find: `#PermitRootLogin prohibit-password`
- Change to: `PermitRootLogin no`
4. Save changes: `Ctrl+X`, then `y`, then `Enter`.
5. Go to ssh_config:
```bash
nano /etc/ssh/ssh_config
```
6. Change port 22 to 4242.
7. Restart the ssh service so it can be updated:
```bash
sudo service ssh restart
sudo service ssh status  # verify
```

## How to install UFW
1. Install the UFW package:
``bash
sudo apt install ufw
```
2. Enable the firewall:
```bash
sudo ufw enable
```

## How to allow a port to Firewall
```bash
sudo ufw allow <port>
sudo ufw status  # verify
```
```bash
sudo ufw allow 4242
```

## How to configure sudo policies (with Nano)
1. Create `sudo_config`:
```bash
touch /etc/sudoers.d/sudo_config
```
2. Create `/var/log/`:
```bash
mkdir /var/log/sudo
```
3. Edit `sudo_config`:
```bash
nano /etc/sudoers.d/sudo_config
```
4. Set it up with these commands:
```bash
Defaults  passwd_tries=3
Defaults  badpass_message="Incorrect password."
Defaults  logfile="/var/log/sudo/sudo_config"
Defaults  log_input, log_output
Defaults  iolog_dir="/var/log/sudo"
Defaults  requiretty
Defaults  secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
```
- `passwd_tries` - total tries for entering the sudo pass.
- `badpass_message` - message for when the password fails.
- `logfile` - path where the sudo logs will be stored.
- `log_input, log_output, ilog_dir` - what will be loged.
- `requiretty` - TTY become required.
- `secure_path` - folders that will be excluded of sudo.

## How to configure password policies
1. Edit the `login.defs` file:
```bash
nano /etc/login.defs
```
2. Modify password parameters:
- Change: `PASS_MAX_DAYS 99999` to `PASS_MAX_DAYS 30`
- Change: `PASS_MIN_DAYS 0` to `PASS_MIN_DAYS 2`

### Installing password quality enforcement
1. Install password quality library:
```bash
sudo apt install libpam-pwquality
```
2. Edit PAM (Pliggable Authentication Modules) configuration:
```bash
nano /etc/pam.d/common-password
```
Add these commands after `retry=3`:
```bash
minlen=10 ucredit=-1 dcredit=-1 lcredit=-1 maxrepeat=3 reject_username difok=7 enforce_for_root
```
- `minlen=10` - min characters the password must contain.
- `ucredit=-1` - at least one uppercase letter (- min, + max).
- `dcredit=-1` - at least one digit.
- `lcredit=-1` - at least one lowercase letter.
- `maxrepeat=3` - the password cannot have the same char repeated three times in a row.
- `reject_username` - the password cannot contain the username itself.
- `difok=7` - the password must contain at least 7 different chars from the last password used.
- `enforce_for_root` - apply these rules for root too.

## Script
- `uname -a` - shows architecture info (Linux, Debian, etc).
- `grep processor /proc/cpuinfo | wc -l` - shows the number of cores used.
- `free --mega | awk '$1 == "Mem:" {print $3}'` - shows the number mb of used memory.
- `free --mega | awk '$1 == "Mem:" {print $2}'` - shows the total mb memory.
- `free --mega | awk '$1 == "Mem:" {printf("(%.2f%%)\n", $3/$2*100)}'` - shows the percentage of used memory.

































