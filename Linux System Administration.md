# Filesystem
The Linux filesystem is hierarchical meaning at the top level is the root directory `/`. All other directories are subdirectories of the root directory `/`. Under the root directory no individual files are stored, these are kept in the different subdirectories.

>[!note]
>System files are protected from user modification and can only be modified by the root user.
>Users can only modify their own home directories, the `/tmp` directory and shared directories specifically created and modified by the admin.

| Directory | Description                                                         |
| --------- | ------------------------------------------------------------------- |
| `/`       | root filesystem, only contains other directories                    |
| `/bin`    | binaries directory, contains executable files. Points to `/usr/bin` |
| `/dev`    | device directory, contains device files to address periherals       |
| `/etc`    | Contains system, configuration files for users and services         |
| `/home`   | Users' home directories                                             |
| `/lib`    | System libraries files, points to `/usr/lib`                        |
| `/media`  | Directory for mounting media (USB drives, DVD disks, ...)           |
| `/mnt`    | mount directory for mounting remote filesystems                     |
| `/opt`    | Directory in which third-party software is installed                |
| `/proc`   | virtual filesystem that tracks system processes                     |
| `/root`   | Root user's home directory                                          |
| `/run`    | Variable and volatile runtime data                                  |
| `/sbin`   | System binary (executable) files                                    |
| `/srv`    | Might contain data from system services                             |
| `/sys`    | Contains kernel information                                         |
| `/tmp`    | Directory for storing session information and temporary files       |
| `/usr`    | Programs and libraries for users and user-related programs          |
| `/var`    | Variable files such as logs, spools and queues                      |
# Command Line Interface (CLI)
With the CLI you can interact with the system using commands that are entered with a keyboard or *standard input (stdin)*. The source from stdin can be a file redirection, programs and other sources. 
The output to these commands are displayed on the screen or *standard output (stdout)* and when an error occurs you receive a *standard error (stderr)*.
 Basic Commands:
 - `pwd`: "print working directory"
   displays where you currentls are on the filesystem
```
$ pwd
/home/user1
```
 - `cd`: "change directories"
   places you into another directory
```
# Change into /etc directory
$ cd /etc

# Change to /bin directory with absolute path
$ cd /usr/bin

# Go out of a subdirectory
$ cd ..

# Go to the home directory
$ cd
```
- `ls`: "list"
  lists the content of a specific directory or your current directoy
```
# List content of a specific directory
$ ls /usr/bin
a2x       getcifsacl    p11-kit            snmpping 
a2x.py    getconf       pack200            snmpps 
ac        getent        package-cleanup    snmpset 
...

# List content of current directory
$ ls

# List all content/hidden contents
$ ls -a
```

# Regular and Privileged Accounts
## Regular User
Regular user have almost unrestricted power in their own home directory to create, modify, remove and manipulate files, but outside of their directory they have almost no power. 

## Root User
The root user has unrestricted permission to create, edit, move or remove any file on the system. They can also reboot, change runlevels and shout down the system.
There are three ways to become a root user:
- Logging in as the root user
- Using the `su` command
- Using the `sudo` command

### Logging in as Root
On some Linux distributions you can log in directly as the root user over SSH or on the console. Both are not recommended, allowing access over the network via SSH could allow attackers to brute force a root login and logging in on the console prevents the system from logging which user logged in as root. 
Recording who uses the root user account is important if something goes wrong, you want to know which administrator performed the actions.
### Using the su Command
A user can use the `su` (substitute user) command to become the root user, but this requires the user to know the root password, which could pose a security problem.
```
$ su root
Password:
#
```
>[!note]
>The root user can also use `su` to login as any other user account without knowing the user's password.
>Regular users can also use `su` to login as other users, but they need to know the password of that account.
>

A better way is to use `su -`, with `-` specifying that you want to use the root user's full environment and not just the account privilege. 
### Using the sudo Command
The best method for a *sudoer* (a user account configured for `sudo` use) to obtain root access is to use the `sudo` ("substitute user do") command. The command must precede each command issued and on first use the sudoer must enter their password.

#### Creating a Sudoer
To create a sudoer you must have root access and edit the `/etc/sudoers` file with the `visudo` utility. 
You can configure the sudoer with restrictive permissions (f.e. run a single command as root) or permissive (run any command as root).

# Reading and Modifying Permissions
## Read, Write and Execute
The three file permissions are:
- *r*: Read
  View a file or list directory contents
- *w*: Write
  Create and modify a file or copy, move and create files in a directory
- *x*: Execute
  Execute/run a file or cd into a directory

Using the `rwx` designation to identify permissions is known as *symbolic mode*.

>[!note] 
>On Linux a file is executable or not based on its permission, opposed to the name/ending on Windows (f.e. `.exe` files).

## Numerical Permission Values
Each permission mode has its own assigned numerical value:

| Permission Mode | Value |
| --------------- | ----- |
| Read            | 4     |
| Write           | 2     |
| Execute         | 1     |
| None            | 0     |
## Group Permissions
You can globally assign permissions to a file with group permissions, which apply to a user's group:

| Permission Group | Value |
| ---------------- | ----- |
| User             | u     |
| Group            | g     |
| Other            | o     |
| All              | a     |
## Reading Permission
The permissions of a file or directory are displayed as 10 characters (`----------`) with each position indicating the permission and the permission group.

The first position is the *special character* and indicates special file types (d.e. directories with a `d`). Regular files are indicated by a `-`.
```
# Directory
d---------

# Regular file
----------
```

The three positions after the special character are for user permissions, the position 5-7 are for group permissions and the last three are for other users.
The permissions themselves are indicated by `r`,`w` and `x`. 

As an example lets create a new file with `touch file.txt` and with `ls -l` check the permissions:
```
-rw-rw-r--
```
- As stated above the first position is a `-` meaning it is a regular file
- The first triad is `rw-` meaning the user can read and write, but not execute it
- The second triad `rw-` means that the group can do the same as the user
- The last three positions (`r--`) mean that others can only read, but not write or execute the file

The permissions could also be display as numerical values. Looking at the table above in **Numerical Permission Values** we would have:
- `rw-`: `6` 
- `rw-`: `6`
- `r--`: `4`

The permissions of the file would then be displayed as `664`

As a summary look at the illustration:
![[Permissions.png]]

## Change File Permissions
You can change the permission of a file with the `chmod` command and either the symbolic or numeric permission values.

>[!note]
>The default permission is `-rw-rw-r--` in symbolic values and `664` in numeric values.

When using the *symbolic values* you have to specify the group (`ugo`) and then add (`+`) or subtract (`-`) the permission value. 
```
# Subtract the reading permission form "other" (o)
# -rw-rw-r--
$ chmod o - r file.txt
=> -rw-rw----

# Add execution to a Shell script for the user
# -rw-rw-r--
$ chmod u + x file.sh 
=> -rwxrw-r--

# Add and subtract multiple permissions
# -rw-rw-r--
$ chmod ug + x, o + w file.txt
=> -rwxrwxrw-

$ chmod a-x, o-rw file.txt
=> -rw-rw---

# -rw-rw-rw-
$ chmod a + x, o - rw file.txt
=> -rwxrwx--x
```

When using *numeric values* you simply calculate the permission values and assign them to the file.
```
# Subtract the reading permission form "other" (o)
$ chmod 660

# Add execution to a Shell script for the user
$ chmod 764

# Add and subtract multiple permissions
# Add execution permission to user and group, add writing permission to other
$ chmod 772

# Subtract execution permission from user and read/write permission from other
$ chmod 660

# Add execution permission to all, subtract read/write permission from other
$ chmod 771
```

## Default Permissions: umask
The *user file-creation mask (umask)* is a global setting that filters or masks certain permissions when a file is created. 
The default umask is `0002`, with the first position being used for special permissions ( `setuid`, `setgid`, `sticky`) and the last three are for the `user`, `group` and `other`.

The value corresponds with the numerical permission values, a value of `2` in the umask will filter out the writing permission, meaning that by default `other` can not alter the file.

A umask of `0022` would filter out writing for both `group` and `other` and a umask of `0006` would block all permissions for `other`.

>[!note]
>The execution permission is never set by default when a file is created, you have to set this permission manually.

To alter the umask for a user you can calculate the permissions and then append it to the `.bashrc` file that resides in the user's home directory.
```
$ echo umask 006 >> ~/.bashrc
```

# Customizing the User Environment
You can make changes to the user's environment, by changing the `.bashc` and `.bash_logout` files. When you log into a Linux system the `.bashrc` file executes first and after that the `.bash_profile` file. Upon logout the `.bash_logout` file will execute.

## /etc/bashrc
This file is the first personalization file to execute when you interactively log into a Linux system and also executes on interactive nonlogin shells. It provides the bash shell with functions and aliases, which is why this file should not be edited.

>[!tip] .bashrc
>The `/etc/profile` is analogous to the `.bashrc` file in a user's home directory. If you need to make changes, change them there.
## /etc/profile
This file is a system-wide startup file, that provides generic variables, paths and other settings to all users. This file should not be edited.

>[!tip] .bash_profile
>The `/etc/profile` file is analogous to the `.bash_profile` file in a user's home directory. If you want to make changes, change them there.

## .bashrc
This file is located in a user's home directory and is used for setting and including functions as well as setting aliases for commands. 
You should change your PATH in this file so your shell behaves similarly for login and nonlogin instances.

>[!note] Aliases
>Aliases are shortcuts to commands and their options.
>To create a version of commands the ask you to verify before it performs the action:
>```
>alias rm = 'rm -i'
>alias cp = 'cp -i'
>alias mv = 'mv -i' 
>``` 
>The `-i` option means "interactive" and is handy for destructive commands like `rm` (remove).

## bash_profile
Customizations in this file will run in interactive login shells, but will not be available for interactive, nonlogin shells.

## .bash_logout
This file executes upon logout and can be used to clean up temporary files, log time using the shell or top send a message upon logout.

## /etc/skel Directory
This directory is used for holding files that are placed in the home directory of every account upon creation.
If you create a new file, existing users will not receive the file.

## Customizing the Shell Prompt
To set a custom shell prompt you can overwrite the `(PS1): PS1="[\u@\h \W]\\$ "` variable in the `~/.bashrc`
![[escape characters shell prompt.png]]
# Managing Users
## User and Group ID Numbering Conventions
When a new user account is created, the systems assigns them a *user ID (UID)* and a *group ID (GID)*.
UIDs and GIDs usually follow the conventions displayed below

| UID   | GID   | Description             |
| ----- | ----- | ----------------------- |
| 0     | 0     | Root                    |
| 1-999 | 1-999 | System/service accounts |
| 1000+ | 1000+ | User accounts           |
## Creating User Accounts
### useradd
You can add users with the the `useradd` command
```
# useradd jsmith
```
This command creates a home directory `/home/jsmith` and places an entry into `/etc/passwd`.

You can also add more information, which are then stored in a field in the  `/etc/passwd` file.
```
# useradd -c "Jane Smith,Room 26,212-555-1000,jsmith@example.com" jsmith
```
The `-c` option stores the information in the comment field (the fifth field).
The fields of `/etc/passwd` are:
-  Username 
- /etc/shadow password field 
- User ID 
- Primary group ID 
- The comment field 
- Home directory 
- Default shell

>[!note]
>Passwords are stored in `/etc/shadow` file, not in `/etc/passwd`.

> [!important]
> After you created the account you have to create a initial password with `passwd account-name` otherwise the user will not be able to log into the account:
> ```
> # passwd jsmith
> ```

### adduser
Some distributions `adduser` have interactive Pearl script, which guides you through the account creation process. 
## Modifying User Accounts

>[!note]
>It is advised that if there is a tool for a action or task, you should use it rather than editing configuration files.

To modify a user account use the `usermod` command, with which you can:
• Add the user to a supplementary group
• Change the user’s comment field in /etc/passwd
• Change the user’s home directory
• Set an account expiry date
• Remove an expiry date
• Change a user’s login name (username)
• Lock/unlock a user’s account
• Move the contents of a user’s home directory
• Change a user’s login shell
• Change a user’s ID

### Add a Supplementary Group
When a user is part of multiple groups or departments (finance, development, engineering, ...) with their own shared directories, you can use `usermod -a -G GID/group-name user-name` to grant the user access to the resources.
```
# usermod -a -G 8020 jsmith
# usermod -a -G finance jsmith
# usermod -a -G 8342, 8901 jsmith
```
>[!note]
>The `-a` (append) and `-G` (supplementary group) options have to be used together, if you only use `-G` the user will be removed from all other groups except the one specified in the command.

### Changing User Comments Field
To change the user comments (GECOS) field, to add information, you can use `chmod -c comment account-name`
```
# usermod -c "Jane Smith-Jonson" jsmith
```

You could also use `chfn`
```
# chfn -f "Janie Smith" jsmith
```
This command changes the user's full name field, there are also:
- *-o*: Office
- *-p*: office phone
- *-h*: home phone
### Setting an Expiration Date on an Account
To set an expiration data on an account use `usermod -e YYYY-MM-DD account-name`
```
# usermod -e 2025-07-13
```
### Changing a User's Login Shell
To change the default bash shell to a different shell you can use `usermod -s shell username`
```
# usermod -s /bin/sh jsmit
```

You could also use `chsh -s shell`
```
$ chsh -s /bin/zsh
```

### Removing User Accounts
To remove a user you can use `userdel username`
```
# userdel jsmith
```
 This command will delete the user's entry from `/etc/passwd` and from `/etc/shadow` but will leave the user's home directory intact.

If you also want to delete the user's home directory and all contents inside it use `userdel -r username`
```
# userdel -r jsmith
```

### Password Changes
You can use the `chage` command to configure passwords and how often they have to be changed and more.
To list the password configurations use `chage -l username`
```
# chage -l jsmith
```

If you want to set a password change every 90 days and a minimum number of days between password changes to 1 use `chage -m minimum-days -M maximum-days username`
```
# chage -m 1 -M 90 jsmith
```

## Managing Groups
Group Management allows you to do the following:
• Manage permissions on assets such as folders and files
• Manage permissions according to job function
• Change permissions for a large number of users rather than for each
user individually
• Easily add users to and remove users from a group’s shared folders and files
• Restrict permissions to sensitive folders and files

**Commands**:
- Add a group with `groupadd groupname`
	```
	# groupadd finance
	```
- Delete a group with `groupdel group-name`
  ```
	# groupdel finance 
	```
- Rename a group using `groupmod -n new-name old-name`
  ```
	# groupmod -n finance1 finance
	```
- Add user with `usermod -a -G group username` 
  ```
	# usermod -a -G finance jsmith
	```
 - Delete users from groups with `gpasswd -d user-name group`
   ```
	# gpassed -d jsmith finance
	```

# Networking and Security
## Pruning the System
Pruning means to remove any unnecessary services and daemons from a system. 
You should only install what you need to provide services to users and other systems. 
Services that are only occasionally used or leave the system on a vulnerable state should only be activated when used and shut down when no longer used.
## Securing Network Daemons
The simplest method to secure network daemons is to use secure versions of the services you want to provide, f.e. using DNSSEC for a DNS server, LDAPS for Ligthweight Directory Access Protocol (LDAP) server and HTTPS for web server.
Some secure services are:
![[Secure Services Network Daemons.png]]
## Secure Shell Daemon
The Secure Shell Daemon (SSHD) is available on almost every Linux system and is the most common network daemon. It has built-in security, but is still vulnerable so further methods are needed to reduce the damage potential attacks can cause.
### Limiting access to SSHD from specific hosts
To allow or deny access to any daemon you can add an entry in the `/etc/hosts.allow` and `/etc/hosts.deny` files, but the first is more important as it overwrites the settings in the `hosts.deny` file.
To enable SSH connectivity from one IP and block all other IPs from connecting write:
```
sshd: 192.168.1.20
sshd: ALL: DENY
```

>[!tip]
>You can also isolate the system by subnet using 192.168.1.0/24 instead of individual IPs.
>

> [!hint] 
> If such an entry in `hosts.allow` does not work you need to check for `tcp_wrapper` integration in the SSHD.
> ```
>$ sudo ldd /usr/sbin/sshd | grep libwrap 
>```
>If you receive no response, consider the following:
>- Use other mehtods to secure SSHD (firewall rules, iptables, nftables)
>- Compile `openssh_server` with `tcp_wrapper` enabled
>- Find and replace the current SSHD with an `openssh_server` package that has `tcp_wrapper` enabled.
### Implementing firewalld, iptables and nftables rules
#### firewalld
1. Delete the ssh service from firewalld's rules:
   ```
	$ sudo firewall-cmd --permanent --remove-service=ssh
	```
2. Add a new zone 
   ```
	$ sudo firewall-cmd --permanent --new-zone=SSH_zone
	$ sudo firewall-cmd --permanent --zone=SSH_zone --add-source=192.268.1.50
	$ sudo firewall-cmd --permanent --zone=SSH_zone --add-service=ssh
	``` 
3. Reload the firewall to make the new configuration active:
   ```
	$ sudo firewall-cmd --reload
	```
#### iptables
Restrict SSH access with:
```
$ sudo iptables -A INPUT -p tcp -s 192.168.1.50 --dport 22 -j ACCEPT
```
#### nftables
> [!NOTE]
> The *netfilter (nft)* system replaces `iptables` and combines `iptables`, `ip6tables`, `arptables` and `ebtables` into one utility. 

 - Add a new rule:
```
$ sudo nft insert rule ip filter input ip saddr 192.168.1.50 tcp dport 22 accept
```

> [!error]
> If you receive the following error you must create a new table and chain for the rule or use an existing chain.
> `Error: Could not process rule: No such file or directory`
> 
> Use the existing chain `INPUT`, remember that chain names are case sensitive
> ```
> $ sudo nft insert rule ip filter INPUT ip saddr 192.168.1.50 tcp dport 22 accept
>```
> Create a new rule and chain
> 1. Create a new table
>    ```
> 	$ sudo nft add table ip filter
> 	```
> 2. Create Chain
>    ```
> 	$ sudo nft add chain ip filter input { type filter hook input priority 0\; }
> 	```
> 3. Add the new rule
>    ```
> 	$ sudo nft insert rule ip filter input ip saddr 192.168.1.50 tcp dport 22 accept
> 	```
> 	
- Restart `nftables`
```
$ sudo systemctl restart nftables
```

### Denying SSH access for the root user
It is important to deny SSH access to the root users, some Linux systems deny it by default, while other do not. 
Check `/etc/ssh/sshd_config` for the following line:
```
PermitRootLogin yes
```
Change "yes" to "no" and restart the SSH service:
```
$ sudo systemctl restart sshd
```

### Using Keys instead of Passwords
Using key files for authentication is more secure than password authentication.
On the remote host:
1. To enable key authentication over SSH you first have to change the "yes to "no" in the `/etc/ssh/shhd_config` file:
	```
	PasswordAuthentication yes
	```
2. Then check the following lines to ensure they are uncommented and set as shown:
	```
	PubkeyAuthentication yes
	Authorized KeysFile .ssh/authorized_keys
	```
3. Next restart the SSH service
	```
	$ sudo systemctl restart sshd
	```

On the client or on the local system you have to generate a public/private key pair:
```
$ shh-keygen -t rsa
```
Then copy the key to the remote host:
```
$ ssh-copy-id user@ip-address
```

After these steps try to loggin into the remote system:
```
$ ssh user@ip-address
```

# Installing and Uninstalling Software
## Updating the System
To update a Debian-based system use:
```
$ sudo apt update
```
## Un-/ Installing Software from Repositories
Installing software from repositories is the easiest method of installing software, as the repository contains all dependencies, gathers and installs them.
To install the text-based Lynx browser on an Ubuntu system use:
```
$ sudo apt install lynx
```
### Uninstall
To uninstall the browser use:
```
$ sudo apt purge lynx
```
>[!note]
>`purge` only removes the `lynx` package, leaving the dependencies on  your system.
>To erase those files you have to use `sudo autoremove`.
>To remove the binaries but leaving the configuration and other files intact use `remove`.

## Un-/ Installing Individual Software Packages
When installing packages from sources like GitHub or SourceForge you use local package manager utilities such as `rpm` and `dpkg`.
>[!note]
>The `downloadonly` option downloads the package without installing it so we have to install them manually at the command line.

On a Ubuntu system:
Download the package:
```
$ sudo apt install --download-only lynx
```
Using this method the downloaded package will be stored in `/var/cache/apt/archives` as a `.deb` package from which the package can be installed.

>[!note]
>The system will not allow you to install the package without installing the dependencies that can be found in the same directory:
>```
>$ ls -1 /var/cache/apt/archives
>libidn11_1.33-2.2ubuntu2_amd64.deb 
>lynx_2.9.0dev.5-1_amd64.deb 
>lynx-common_2.9.0dev.5-1_all.deb
>```

Install the dependencies and then the package
```
$ sudo dpkg -i libidn11_1.33-2.2ubuntu2_amd64.deb

$ sudo dpkg -i lynx-common_2.9.0dev.5-1_all.deb

$ sudo dpkg -i lynx_2.9.0dev.5-1_amd64.deb
```
### Uninstall
To uninstall a package you also have to uninstall it's dependencies. In the process you uninstall the dependencies first and then the package itself.
```
$ sudo apt purge lynx

$ sudo apt purge lynx-common

$ sudo apt purge libidn11
```

### Finding Package Dependencies
To list the dependencies on an Ubuntu system use:
```
$ sudo apt show lynx
```

## Un-/ Installing Software from Source Code
>[!warning]
>You must satisfy dependencies for the software you install when installing and updating, if a previous version is not fully overwritten or removed version conflicts can occure.
>You also need to install a full complement of development tools.

### Building a Development Environment
Install a code compiler and supporting software on a Ubuntu system with:
```
$ sudo apt install build-essential
```

### Download, Extract and Install Software
Download the compressed source code:
```
$ wget https://invisible-mirror.net/archives/lynx/tarballs/lynx2.8.9rel.1.tar.gz
```

Extract the source code from the compressed archive:
```
$ tar zxvf lynx2.8.9rel.1.tar.gz
```

Change into the directory containing the source code:
```
$ cd lynx2.8.9rel.1
```

>[!note]
>Before running `configure` always read the `README` file in the source code tree.
>

Run the `configure` command:
```
$ ./configure
```
The script fails as there are dependencies needed:
```
configure: error: No curses header-files found
```

Install the dependencies
```
sudo apt install lib32ncurses-dev
```

Run the `make` command
```
$ make
```

To install the software to its proper location and set the correct permissions use:
```
$ sudo make install
```

Remove all object code and other temporary files:
```
$ make clean
```
### Uninstall
If the original `makefile` is still intact you can easily uninstall a package, if you do not want to keep all source trees on your system for every compiled program make a backup.
The `makefile` must be in your current directory when uninstalling:
```
$ sudo make uninstall
```

# Managing Storage
## Mounting and Mount Points

> [!NOTE]
> Disk drives refer to hard disks (HDD), solid-state drives (SSD) and USB thumb drives.

Before accessing a disk on Linux you have to mount it on a directory or *mount point*.
For example if you want to access a disk identified by the system as `/dev/ssd1` you have to create a directory such as `opt/software` and mount it with the `sudo mount` command.
- create a directory
  ```
	$ sudo mkdir opt/software
	```
- mount the drive
  ```
	$ sudo mount /dev/ssd1 /opt/software
	```

> [!NOTE]
> The generic mount point `/mnt` can be used to temporarily mount disks, but it should not be used as a permanent mount point.

To mount disks automatically at boot time, you must create an entry in the `/etc/fstab` file, that includes the disk and the mount point.
The entry for the `/dev/ssd1` is
```
UUID=324ddbc2-353b-4221-a80e-49ec356678dc /opt/software xfs defaults 0 0
```

>[!tip]
>You can use the `blkid` command with the disks name to get its UUID.
## Checking Space
To check the disk space usage you can use `df -h` command, with the `-h` displaying the space as `M` for megabytes, `G` for gigabytes, etc.
```
$ df -h
```

You can also use `du` to check individual directories and what consumes space in it.
```
$ sudo du -h /var/log
```

## Swap Space
 Swap space is a Linux disk partition that is used by the kernel to write inactive programs from memory to disk or activating programs from disk to memory, thus leaving more RAM for active programs.

## RAM-Based Temporary Space (ramfs and tmpfs)
`ramfs` and `tmpfs` are both RAM-based temporary filesystems, whose files are exist in memory but are not written to disk.
`tmpfs` writes temporary files and caches to memory rather than disk and writes files to swap space to save resources.
`tmpfs` replaced `ramfs` and is the default for all contemporary Linux distributions.

## Adding a New Disk to a System
### Installing the Disk
If the the system has hot-swappable interfaces you can simply connect the new disk, but if your system does not have these you shut the system down before installing the new disk.
### Prepping the Disk for Use
First you determine the the disk's device name with:
```
$ sudo fdisk -l
```
Initialize the disk with `sudo fdisk disk-name`
```
$ sudo fdisk /debv/sdb
```
Create a filesystem, f.e. using XFS:
```
$ sudo mkfs.xfs /dev/sdb1
```
Create a directory to use as a mounting point
```
$ sudo kfir /opt/software
```
Mount the disk:
```
$ sudo mount /dev/sdb1 /opt/software
```
 Check if the disk is mounted:
 ```
 $ mount | grep sdb1
```
Edit the `/etc/fstab` to permanently mount the disk
On Ubuntu use:
```
/dev/disk/by-uuid/ca2701e0-3e75-4930-b14e-d83e1... /opt/software xfs defaults 0 0
```
### Implementing Logical Volumes
Identify available unused disks or disks you want to convert:
``` 
$ lsblk
```
Create a physical volume (PV) with `pvcreate disk-name`:
```
$ sudo pvcreate /dev/sdb
```
Create and name a volume group (VG) with `vgcreate VG-name PV-name`:
```
$ sudo vgcreate vgsw /dev/sdb 
```

>[!note]
>Use `pvdisplay PV-name` to display more information about the volume:
>```
>$ sudo pvdisplay /dev/sdb
>```

Create a logical volume (LV) with `lvcreate -L size -n lvname VG-name`
The size must be indicated by a `G` (gigabytes) or a `M` (megabytes)
```
$ sudo lvcreate -L 1G-n software-lv vgsw
```
Create a filesystem
```
$ sudo mkfs.xfs /dev/vgsw/software-lv
```
Create a mount point:
```
$ sudo mkdir /sw
```
Mount the filesystem
```
$ sudo mount /dev/vgsw/software-lv /sw
```
Add an entry to the `/etc/fstab` file:
```
/dev/vgsw/software-lv /sw xfs defaults 0 0
```
### Extending Logical Volumes
Extend the Logical Volume with:
- `lvextend -L +size(M or G) LVname` to extend the volume to a specific size
- `lvextend -l +100%FREE LVname` to extend the volume to the maximum free space

```
$ sudo lvextend -l +100%FREE /dev/vgsw/software-lv
```
Extend the filesystem with `xfs_growfs`, you can specify a size parameter `-D`, not specifying the size will resize the filesystem to the maximum available space
```
$ sudo xfs_growfs /dev/vgsw/software-lv
```

# Cleaning the System
## Cleaning the /tmp Directory
A way of preventing users from storing files in `/tmp` is to schedule a `cron` job that, after a backup of the systemfiles, removes all nonsystem files.

You can also enable `tmp.mount`, which creates a *temporary filesystem (tmpfs)* and mounts it as `/tmp`. `tmpfs` is RAM, so storing files in it is discouraged. 
The steps to enable `tmp.mount` are:
1. Check the `/tmp` directory to see if you need to configure `tmp.mount`
   ```
	$ df -h /tmp
 	```
	 If the directory is a  mounted on `/` enable `tmp.mount`
2. Enable and start `tmp.mount`
   ```
	$ sudo systemctl enable tmp.mount
	$ sudo systemctl start tmp.mount
	```
## Preventing /home from filling up
If `/home` is on the same filesystem as `/`, users could fill `/` and cause system problems, but as of the nature of the files contained in `/home` it can not be replaced by a volatile filesystem.
To prevent this there are two approaches:
Approach one
   -  Shrinking an existing LVM filesystem
   - Creating a partition and filesystem
   - Mounting that filesystem as `/home`

Approach two:
- Install a new disk
- Create a new partition 
- Mount the new partition as `/home`

The steps for the seconf approach are as following:
1. Create or install a new disk
   >[!tip]
>Identify the new disk with 
>`$ lsblk | grep disk`
	
2. Create a new partition
   ```
	$ sudo fdisk /dev/sdc
	```	
	Verify that the partition exists
	```
	$ lsblk | grep sdc
3. Make the filesystem
   For the `/home` directory format the filesystem as `ext4`
   ```
	$ sudo mkfs.ext4 /dev/sdc1 
	```
	Verify that the `ext4` filesystem was correctly created
   ```
	$ lsblk -o NAME,FSTYPE,SIZE | grep sdc   
	```
4. Mount the new filesystem on `/mnt`
   ```
	$ sudo mount /dev/sdc1 /mnt 
	```
	>[!warning]
	>`/mnt` is the temporary mounting point so you can copy all files from `/home` to the disk before mounting it on `/home`. Mounting a disk on an nonempty directory makes the files inaccessible
	
 5. Copy or move all files from `/home` to `/mnt` 
    ```
	$ sudo cp -a /home/* /mnt
	```
	Check if all files are on the disk
	```
	$ ls -la /mnt
	```
6. Remove all files from `/home`
   ```
	$ sudo rm -rf /home/*
	```
	>[!note]
	>Removing all files from the original `/home` releases disk space back to the `/` filesystem
	>
7. Unmount the new partition from `/mnt`
   ```
	$ sudo unmount /mnt
	```
8. Mount the new partition on `/home`
   ```
	$ sudo mount /dev/sdc1 /home
	```
9. Add an entry for `/home` in `/etc/fstab`
    ```
    /dev/sdc1 /home ext4 defaults 0 0
	```	
## Decluttering Shared Directories
## Deduplicating Files with fdupes
With `fdupes` you can search for duplicate files, remove them  and provide a link to one file, or delete them.
You can install it from a repository or directly from GitHub
```
$ git clone https://github.com/tobiasschulz/fdupes
$ cd fdupes
$ make fdupes
$ sudo make install  
```
To check a directory, including its subdirectories and display the size of the files use:
```
$fdupes -rS /opt/shared
```
For more see the [man page](https://linux.die.net/man/1/fdupes) 
## Using Quotas 
Quotas can be used in shared spaces like `/home` to set a limit on memory space users can use. 
You can install `quota` 
```
$ sudo apt install quota
```
>[!note]
>To use `quota` you need to have an `xfs` filesystem.

After installing create two new files in the directory, f.e. `/home`, `quota.user` and `quota.group`
```
$ sudo touch /home/quota.group /home/quto.user
```
Enable quotas with the `quotaon` command:
```
$ sudo quotaon /home
```
Now you can set soft- (`bsoft`), hardlimits (`bhard`) and also how many files a user can create (`isoft`/`ihard`).
```
# sudo xfs_quota -x -c \
'limit -u bsoft=50m bhard=80m isoft=60 \ ihard=80 djones' /home
```
## Securing the System
A single security setting or policy is not possible for multiple different systems, a summary of settings for different systems is below:
![[Securing the system summary.png]]

# Monitoring
## Monitoring System Health 
To check performance and format system statistics you can use [`sysstat`](https://github.com/sysstat/sysstat).
 After installing, edit the `/etc/default/systat` file and change `ENABLED=false` to `ENABLED=true`, then restart the service:
 ```
 $ sudo service sysstat restart
 ``` 
 The package contains different binaries:
 
 ![[Sysstat binaries.png]]
 
 Another tool you can use is `vmstat`.
 Both tools you have to provide a specific number of snapshots and a time delay between them in seconds.
 If you want that the tools take 2 snapshots with 5 seconds between them use:
 ```
 $ iostat 5 2
 $ vmstat 5 2
 ```  

## Gathering System Activity Reports
You can use the [`sar`](https://www.geeksforgeeks.org/linux-unix/sar-command-linux-monitor-system-performance/) command to create system activity reports. You also have to provide a number of snapshots and a delay between them.
```
$ sar 5 5
```
## Formatting System Activity Reports
To capture system data collected by `sar` to a file in a format such as CSV or XML, you can use the  [`sadf`](https://man7.org/linux/man-pages/man1/sadf.1.html) command.

## Tracking CPU Usage
You can get a real-time view of a systems that consume the most CPU resources with the `top` command as well as `atop` and `htop`.
>[!note]
>You can sort the output of `top`:
>![[top CPU sorting.png]]

## sysstat Monitoring
The `sysstat` (System Status) package is a native monitoring tool that is prepackaged on Linux distributions.
The package contains:
- `sar`: system activity reporter
- `sadf`: system activity data formatter that displays data in multiple formats
- `iostat`: displays CPU utilization and disk I/O statistics
- `tapestat` displays tape and tape drive statistics
- `mpstat`: displays multi-processor statistics
- `pidstat`: reports statistics by process ID
- `cifsiostat`: CIFS (Samba/SMB) I/O statistics utility

# Scripting and Automation

