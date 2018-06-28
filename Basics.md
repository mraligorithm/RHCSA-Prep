# Basic Commands 
- - - -
*  `pwd` * Show current working directory path
*  `cd` * Change directory
*  `ls` * List contents of directory
*  `sudo` * Allows a super user to run a command with root priviledges
*  `mkdir` * Create new directory
»»  `-p` * Create parent directories, if do not already exist
*  `rmdir` * Remove directory
*  `rm -rf` * Force remove a directory, recursively (includes all files inside)
*  `touch` * Create new, empty files
- - - -
# Input-Output Redirection
- - - -
* `>` * Redirect standard output to file
	»» `echo “test” > file.txt`
	»» Replaces file, if already exists
* `>>` * Redirects and appends standard output
	»» `echo “test” >> file.txt`
	»» Adds text to bottom of file
* `|` * Chain scripts, files and commands together by the STDOUT as STDIN for the next command
	»» `cat /etc/passwd | grep root`
* `2>` * Redirect standard error
* `2>>` * Redirect and append standard error
* `/dev/null` * Data sent to _dev_null is lost
* `2>&1` * Redirect STDERR to STDOUT
* `<` * Accept input from file
	»» `mysql < filedump.sql`
* `less` * File viewing application and STDOUT can often piped into for ease of reading
* `head` * Show first ten lines of file
	»»  `-n` * Define number of lines
* `tail` * Show last ten lines of file
	»»  `-n` * Define number of lines
- - - -
# File System Hierarchy Standard
- - - -
* `/etc` * Contains configuration files for programs and packages
* `/var` * Variable data specific to system. This data should not be removed or changed when the
system reboots. Logs files tend to be stored within the /var directory
* `/run` * Runtime data for processes since last boot
* `/home` * Location of home directories; used for storing personal documents and information on
the system
* `/root` * root user home directory
* `/tmp` * Files are removed after ten days; universal read/write permissions
* `/boot` * Files needed to start the system boot process
* `/dev` * Contains information on essential devices
- - - -
# Grep and Regular Expressions
* `grep` * Prints lines that match defined pattern
	»» `grep pattern file.txt`
	»» `-i` * Case insensative
	»» -v * Shows lines not containing pattern
* Examples including regex:
	»» `grep linuxacademy filename` * Search for linuxacademy in filename
	»» `grep “^linuxacademy” filename` * Search for lines starting with linuxacademy
	»» `grep “linuxacademy$” filename` * Search for lines ending with linuxacademy
	»» `grep “^[abd]” filename` * Search for characters not contained in brackets
	»» `grep [lL]inuxacademy filename` * Search for pattern starting with either capital or
lowercase L
	»»  `grep “^$” filename` * Search for empty lines
	»»  `grep -v ^# filename` * Search for uncommented lines
*  `egrep` * Same as grep, but using extended regular expressions
*  `fgrep` * Interpret pattern as list of fixed strings
- - - -
# Access Remote Systems Using SSH
* Password authentication * Allows user to log in with only a password; considered to be less
secure than using key-based authentication
* `ssh user@server` * Connect to remote host
* `ssh server command` * Issue command on remote host without connecting
* `scp filename user@server:~/` * Secure copy file to server
* `sftp user@server` * Secure File Transfer Protocol
	»» `?` * Display all options
	»» `ls` * List files
	»» `cd` * Mode directories
	»» `get` * Download	
	»» `quit` * Exit sftp
- - - -
# Log In and Switch Users in Multi-User Targets
- - - -
* Target * Systemd configuration files used for grouping resources
* Interactive shell * Any shell that has a prompt for user interaction
* `su` * Log in as another user
	»» `su user` * Log in to an interactive, non-login shell
	»» `su - user` * Log in to a login shell
* GNU Bourne-Again Shell * Bash
	»» Interactive shell uses either $ (user) or # (root) 	prompt
	»» Takes commands, which run programs
- - - -
# # Archive and Compress Using tar, star, gzip and bzip2
* `tar` * Archive files; does not handle compression
	»» `-c` * Create new archive
	»» `-t` * List contents of archive
	»» `-x` * Extract files from archive
	»» `-z` * Compress or uncompress file in gzip
	»» `-v` * Verbose
	»» `-j` * Compress or uncompress file in bzip2
	»» `-f` * Read archive from or to file

»» Examples
	— `tar -cf helloworld.tar hello world` * Archive hello and world files into helloworld.tar archive
	— `tar -tvf helloworld.tar` * List all files in helloworld.tar archive
	— `tar -xf helloworld.tar` * Extract files in archive
	— `tar -czvf helloworld.tar.gz hello world` * Archive and compress (using gzip) hello and world files into helloworld.tar.gz archive
	— `tar -zxvf helloworld.tar.gz` * Uncompress (in gzip) and extract files from
archive
* `star` * Archiving utility generally used to archive large sets of data; includes pattern-matching
and searching
	»» `-c` * Create archive file
	»» `-v` * Verbose output
	»» `-n` * Show results of running command, without executing the actions
	»» `-t` * List contents of file
	»»  `-x` * Extract file
	»»  `—diff` * Show difference between files
	»»  `-C` * Change to specified directory
	»»  `-f` * Specify file name
»» Examples”
	—  `star -c f=archive.tar file1 file2` * Archive file1 and file2 into archive.tar archive
	—  `star -c -C /home/user/ -f=archive.tar file1 file2`* Move to _home_user and archive file1 and file2 from that directory into archive.tar
	—  `star -x -f=archive.tar` * Extract archive.tar
	—  `star -t -f=archive.tar` * List contents of archive.tar
*  `gzip` * Compression utility used to reduce file sized; files are unavailable until unpacked;
generally used with tar
	»»  `-d` * Decompress files
	»»  `-l` * List compression information
»» Examples:
	—  `gzip file1` * Compress file1 into file1.gz
	—  `gzip -d file1.gz` * Unpack file1
	—  `gunzip filename` * Unpack filename
- - - -
# Create and Edit Files
* `vi` * Text editor that is always installed and useable; replaced vim
* `vim` * Vi iMproved; full-featured version of vi
* `nano` * Simple text editor
* `touch` * Create empty file
- - - -
# Create, Delete, Copy and Move Files and
Directories
* `mkdir` * Make directory
	»»  `-p` * Create parent directories, if not already created
* `cp` * Copy files and directories
	»»  `-R` * Copy directory recursively
* `mv` * Move files and directories
* `rm` * Remove files and directories
	»»  `-r/ -R` * Remove recursively
	»»  `-f` * Force remove
	»»  `-i` * Prompt before removal
- - - -
# Create Hard and Soft Links
* `ln` * Create links between files
	»» Without the `-s` flag, creates a hard link
	»» `-s` * Symlink files
* `symlinks` * Soft links that connects one file to another, symbolically; if the target file moves to
changes, the symlink continues to try use the previous location and must be updated
* `Hard link` * Links directly to an inode to create a new entry referencing an existing file on the
system
- - - -
# List, Set and Change Standard Permissions
* Two ways to define permissions on a standard Linux system:
	»» Using symbolic characters, such as u, g, o, r, w and x
	»» Using octal bits
	»» The RHCSA only requires knowledge of the symbolic
* `chmod` * Change mode; set the permissions for a file or directory
	»» u * User
	»» g * Group
	»» o * Other
	»» a * All
	»» r * Read
	»»  w * Write
	»»  x * Execute
	»»  s * Set UID or GID
	»»  t * Set sticky bit
	»»  -X * Indicate the execute permissions should only affect directories and not regular files

»» Octal bits:
	— 1  * Execute
	— 2  * Write
	— 4  * Read
* `chown` * Change owner and group permissions
	»» `chown user:group filename`
	»» `-R` * Set ownership recursively
* `chgrp` * Change group ownership
* `setuid` * Set user ID permissions on executable file
* `setgid` * Set group ID permissions on executable file
* `umask` * Set default permissions for new directories and files
- - - -
# # Locate, Read and Use System Documentation
* `command` —help
* `info` * Read information files; provides more information than man
* `which` * Show full path of command; useful for scripting
* `whatis` * Display manual page descriptions
* `locate` * Locate files on system by name
* `updatedb` * Update locate command databases
* `man` * Documentation
»» Nine sections:
— 1 * Executable programs and shell commands
— 2 * System calls
— 3 * Library calls
— 4 * Special files
— 5 * File formats
— 6 * Games
— 8 * root user commands
— 7 * Miscellaneous
— 8 * root user commands
— 9 * Kernel routines
* `apropos` * Search man pages and descriptions for text
- - - -
# Boot, Reboot and Shut Down a System
- - - -
	* Reboot:
    »reboot
    `systemctl reboot`
	 `shutdown -r now`
	* Shutdown:
	`systemctl halt`
	`halt`
	`shutdown -h now`
	`init 0`
	* Power off:
	`systemctl poweroff`
	`poweroff`
	`shutdown -P`
- - - -

#linux/commands