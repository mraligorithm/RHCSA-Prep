# Exercises 01
- - - -
When a process creates a directory or file, the respective object-created permissions are set by the process. This means that if a text editor creates a file, it creates it with required permissions. It does not need to execute permissions to create the file. When working with umask, it is safe to assume that you are masking those default permissions, with files starting at 666 and directories starting at 777. When you use umask, you are "masking" the default permissions.
- - - -


1. View current umask permissions and then, for the current shell session, set umask permissions to 0.

```bash
    [root@localhost ~]# umask
    0022
    [root@localhost ~]# umask 0
    [root@localhost ~]# umask
    0000
```
Note: umask adds leading zeros

2. Navigate to the /tmp directory and touch file1 and mkdir dir1. View current permissions.

```bash
    [root@localhost user]# cd /tmp
    [root@localhost tmp]# touch file1
    [root@localhost tmp]# mkdir dir1
    [root@localhost tmp]# ls -l
    total 0
    drwxrwxrwx. 2 root root 6 May  1 11:10 dir1
    -rw-rw-rw-. 1 root root 0 May  1 11:10 file1
```

3. Mask permissions for the "other" users to write a file when created, then touch file2 and view permissions.

Tip: If file permissions start at 666, and you want to "remove/mask" permissions for other users to write, then you need to subtract the octoal notation representing write permissions, which is 2.

```bash
    [root@localhost tmp]# umask 002
    [root@localhost tmp]# touch file2
    [root@localhost tmp]# ls -l
    total 0
    drwxr-xr-x. 2 root root 6 May  1 11:10 dir1
    -rw-rw-rw-. 1 root root 0 May  1 11:10 file1
    -rw-rw-r--. 1 root root 0 May  1 11:13 file2
```

4. Mask write access for group members and the write for "other" permissions, then touch file3 and view permissions.

```bash
    [root@localhost tmp]# umask 022
    [root@localhost tmp]# touch file3
    [root@localhost tmp]# ls -l
    -rw-r--r--. 1 root root 0 May  1 11:16 file3
    [root@localhost tmp]# 
```

5. Mask read and write permissions for the owner of a file, then touch file4 and mkdir dir3 and view permissions.

```bash
    [root@localhost tmp]# umask 600
    [root@localhost tmp]# touch file4
    [root@localhost tmp]# mkdir dir3
    [root@localhost tmp]# ls -l
    total 0
    d--xrwxrwx. 2 root root 6 May  1 11:18 dir3
    ----rw-rw-. 1 root root 0 May  1 11:18 file4
```

6. Mask all permissions, including execute permissions, on new directories, then touch file5.

```bash
    [root@localhost tmp]# umask 777
    [root@localhost tmp]# touch file5
```

    Tip: Setting umask 666 would mask all permissions on files but leave execute on directories. Directories need execute permissions for someone to change into the directory.

7. Mask read/write access for group for non-privileged users and other permissions and make these changes persistent.

```bash
    [root@localhost ]#vim /etc/bashrc
    if [ $UID -gt 199 ] && [ "`id -gn`" = "`id -un`" ]; then
        umask 066
    else
        umask 022
    fi
    [root@localhost ]#vim /etc/profile
    if [ $UID -gt 199 ] && [ "`id -gn`" = "`id -un`" ]; then
        umask 066
    else
        umask 022
    fi
```

    Note: For users to have sudo privileges (be a privileged user), they generally have a primary group of "wheel". What the script says, is if the primary_effective group does not match the username (and remember generally users primary_effective group will be the same as the username), then consider the user a privileged user.
- - - -
# Archiving and Compressing the file 


1. While working in the user root's home directory, create a tar archive of the entire _var_log directory and name the tar file "logs.tar".

    `[root@localhost ~]# tar -cvf logs.tar /var/log`

2. List the contents of the tar archive into standard output.

    `[root@localhost ~]# tar -tf logs.tar`

3. Using gzip, compress the tar file.

    `[root@localhost ~]# gzip logs.tar`

4. Extract the contents of the "log.tar.gz" directory into _root_var/log.

    `[root@localhost ~]# tar -zxvf logs.tar.gz`

5. Using star, create an archive of the contents of the newly-created log directory in _root_var_log into a file called "user-logs.tar". Be sure to preserve the entire path structure so that the archive indicates exactly where the file belonged. i.e. /root_var/log should preceed every file in the archive.

```bash
    [root@localhost ~]# yum install star
    [root@localhost ~]# star -c /root/var/log f=user-logs.tar 
    star: 205 blocks + 0 bytes (total of 2099200 bytes = 2050.00k).
```

6. List the contents of the tar file.

    `[root@localhost ~]# star -t f=user-logs.tar`

7. Compress the star archive into a bzip2 compressed file.

```bash
    [root@localhost ~]# yum install bzip2
    [root@localhost ~]# bzip2 user-logs.tar
```

8. Decompress the star archive into the /root home directory.

    `[root@localhost root]# star -bz -x f=user-logs.tar.bz2`
- - - -
#  Finding Files with locate and find
1. Download and install the locate utility.

    `[root@anthony1 ~]# yum install mlocate`

2. Using the locate utility, search for the "motd" file on the system.

```bash 
    [root@localhost ~]# locate motd
    /etc/motd
    ...
```

3. Add a new user on the system called "mary".

    `[root@localhost ~]# useradd mary`

4. Using the find utility, remove all files owned by mary.

	`[root@localhost ~]# find / -user mary -exec rm {} \;`

5. Using the find utility, find all files that were modified in the last 3 days.

    `[root@localhost ]# find / -mtime -3`
- - - -
# Working with Systemd and Targets
1. Install the httpd package.

    `[root@localhost]# yum install httpd`

2. View all active targets on the system.

    `[root@localhost]# systemctl list-units --type=target`

3. View all targets installed on the disk.

    `[root@localhost]# systemctl list-units --type=target --all`

4. Display the current default target.

    `[root@localhost]# systemctl get-default`

5. Change the default target to the multi-user target if the multi-user target is available.

```bash
    [root@localhost]#  systemctl list-units --type=target | grep multi-user.target
    # multi-user.target   loaded active active Multi-User System
    root@localhost]# systemctl set-default multi-user.target
```

6. View all available systemd configuration units.

    `[root@localhost]# systemctl -t help`

7. Find the status of the sshd service.

    `[root@localhost]# systemctl status sshd.service`

8. List all active service unit configuration files.

    `[root@localhost]# systemctl --type=service `

    or

    `[root@localhost]# systemctl list-units --type=service`

9. Determine if the httpd service is active.

    `[root@localhost]# systemctl is-active httpd`

10. Determine if the httpd service is enabled, and, if it is not, enable it.

    `[root@localhost]# systemctl is-enabled httpd`

11. View enabled and disabled settings for all units of the type "service".

    `[root@localhost]# systemctl list-unit-files --type=service --all`

12. List all service unit configuration files, whether they are active or not.

    `[root@localhost]# systemctl list-units --all`
    OR
    `[root@localhost]# systemctl list-units --type=service --all`
- - - -
# Recovering the Root Password

1. Start or reboot a system to get into the boot menu.

2. Press any key to stop the auto selection of a menu item.

3. Ensure the kernel you intend to boot into is highlighted and press the E key to edit the entry.

4. Navigate to the "linux16" kernel line and hit the End key to go to the end of the line.

5. Append `rd.break` to the linux16 kernel line.

6. Hit Ctrl + X to continue.

7. The system boots into an emergency mode that has the `/sysroot` directory mounted as read only.

8. Mount the `/sysroot` directory with read and write permissions.

`mount -oremount, rw /sysroot`

9. Switch into chroot jail and set the /sysroot as the root file system.

`chroot /sysroot`

10. Reset the root password.

`passwd root`

11. Clean up. Make sure that all unlabled files get relabeled during the boot process for SELinux.

`touch /.autorelabel `

12. Exit chroot jail.

exit

13. Exit the initramfs debug shell.

exit

**Troubleshooting Notes**
If the password did not change after reboot:
The `touch /.autorelabel` command was missed or performed incorrectly.
The file system was not mounted as read/write so the changes made were not persistent.
- - - -
# Interrupting the Boot Process to Change the Boot Target

1. Start or reboot a system to get to the boot menu.

2. Press any key to stop the auto selection of a GRUB item.

3. Ensure the kernel you intend to boot into is highlighted and press the E key to edit the entry.

4. Navigate to the "linux16" kernel line and hit the End key to go to the end of the line.

5. Append the new target to the linux16 kernel line.

systemd.unit=rescue.target

6. Continue booting into the system with Ctrl + X.
- - - -
# Pgrep, pkill, kill and jobs
Exercise 1

1. As the root user, create a job running in the background of your current terminal. Execute the script for that program process to be created.

    `[root@localhost]# (while true; do echo "My program" > ~/output.file; done) &`

2. View the current jobs running in the background of your terminal.

```bash
    [root@localhost ~]# jobs
    [1]-  Running   ( while true; do< echo -n "My program" >> ~/output.file; done ) &
```

3. Stop the process from running, without killing the process, using the kill command.

    `[root@localhost]# kill -SIGSTOP %1 (%1 is the job number, if the job was 2 it would be %2)`

4. View the stopped jobs in the background.

```
    [root@localhost ~]# jobs
    [1]+  Stopped     ( while true; do echo -n "My program" >> ~/output.file; done ) &
```

5. Start the process again using the kill command.

    `[root@localhost]# kill -SIGCONT %1`

6. Kill the process without allowing any blocking of the kill command.

    `[root@localhost]# kill -SIGKILL %1`

Exercise 2

1. Download and install the httpd service.

    `[root@localhost]# yum install httpd`

2. Start the httpd service (or ensure that it is running).

    `[root@localhost]# systemctl start httpd`

3. As the root user, grep for all processes that are running as the root user and display the process names.

    `[root@localhost]# pgrep -u root -l`

4. As the user user, start the vi program at the terminal.

    `[user@localhost]# vi`

5. As the root user, in your second terminal window, grep for all processes running under the user "user" and include the process names.

```bash
    [root@localhost ~]# pgrep -u user -l
    3690 dconf-service
    3694 vim
    ...etc additional output cut off.
```

6. As the root user, grep for the "httpd" process.

    `[root@localhost]# pgrep httpd`

7. As the root user, kill all of the "user" user's processes and boot that user from the system.

```bash
    [root@localhost ~]# w
     14:15:59 up 20:55,  4 users,  load average: 0.00, 0.01, 0.05
    USER     TTY        LOGIN@   IDLE   JCPU   PCPU WHAT
    user pts/0     Mon22    2:58m  0.05s  0.05s bash
    [root@localhost ]# pkill -t pts/0 
```

    This kills every process started from the user's terminal, but it does not boot the user. Now find all running processes left, which should either be Bash or SSH.

```bash
    [root@localhost]# pgrep -u user
    [root@localhost]# pkill -u user ssh
```
- - - -
# Nice, renice and ps
1. Ensure that you have the httpd package installed on the system.

	`[root@localhost]# yum install httpd`

2. Ensure the httpd service is not running.

    `[root@localhost]# systemctl stop httpd`

3. Start the httpd service with the most favorable nice possible.

    `[root@localhost]# nice -n -20 httpd`

4. View the current nice of the httpd service using the ps command and grep command together.

    `[root@localhost]# ps axo pid,comm,nice | grep httpd `

    or 

    `[root@localhost]# ps axo pid,comm,nice --sort=-nice | grep httpd`

    This allows you to sort by nice level.

5. Renice all httpd processes and set the nice level to 0.

    `[root@localhost]# renice -n 0 $(pgrep httpd)`
- - - -
# Monitoring and Calculating CPU Load Averages
1. View the system uptime and load average.

```bash
    [root@localhost ~]# uptime
     09:53:07 up 16:32,  3 users,  load average: 1.02, 1.00, 0.69
```

2. View the system uptime and load average in such a way that it also shows what users are logged in to the system and what the user is doing.

```bash
    [root@localhost ~]# w
     09:53:37 up 16:33,  3 users,  load average: 1.01, 1.00, 0.70
    USER     TTY        LOGIN@   IDLE   JCPU   PCPU WHAT
    user  pts/1     09:18    1.00s  0.10s  5.32s /usr/libexec/gnome-terminal-server
```

3. Using the proc file system and wc, display the number of processors your system has. This is important to calculate the load average of the system.

    `[root@localhost]# grep "model name" /proc/cpuinfo | wc -l`

4. Calculate the 1, 5, and 15 minute CPU load averages for the system.

    There are two processes as a result of our previous command.

```bash
    [root@localhost ~]# grep "model name" /proc/cpuinfo | wc -l
    2
```

    For each, processor 1 is 100%. If you have 1 processor and your load average is 1.2 then your load is greater than 100%. If you have 2 processors and your load is 2 then your load is 100%.

```bash
    [root@localhost ~]# uptime
     09:42:20 up 16:21,  3 users,  load average: 1.04, 0.72, 0.35
    Per CPU load average calculation formula: load average / # of cpu
    Per CPU load average calculation 1 Minute load average: 1.04 / 2 = 52%
    Per CPU load average calculation 5 Minute load average: .72 / 2 = 36%
    Per CPU load average calculation 15 Minute load average: .35 / 2 = 17.5%
```

- - - -












#linux/commands