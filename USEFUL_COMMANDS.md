# Other useful commands:
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
