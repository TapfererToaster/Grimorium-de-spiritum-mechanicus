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
## Permission
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
