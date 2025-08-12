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
The root user has unrestricted permission to create, edit, move or remove any file on the system.  They can also reboot, change runlevels and shout down the system.
There are three ways to become a root user:
- Logging in as the root user
- Using the `su` command
- Using the `sudo` command

### Logging in as Root
On some Linux distributions you can log in directly as the root user over ssh or on the console. Both are not recommended, allowing access over the network via SSH could allow attackers to brute force a root login and logging in on the console prevents the system from logging which user logged in as root. 
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