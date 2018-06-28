# User and FileSystem 
- - - -
# Create, Delete and Modify Local User Accounts
* `id` * Print user and group IDs
* `UID` ranges:
	»» 0 * root
	»» 1-200 * System users for Red Hat processes
	»» 201-999 * System users for processed that do not own files
	»» 1000+ * Regular users
* `/etc/passwd` * User login and password information
* `/etc/shadow` * User login and password hash information
* `Primary group` * The main group for a user; all files created by a user are set under this group
* `/etc/groups` * Group member information
* `getent group username` * Show all groups for a user
* `useradd` * Create user
* `usermod` * Modify user
* `userdel` * Delete user
- - - -

# Change Password and Password Aging
* `chage` * Modify amount of days between password changes
	»» -d * Number of days since 1970-01-01 to define password change
	»» -E * Set password expiration date	
	»» -I * Number of days of inactivity before password expiration
	»» -l * Show account aging information
	»» -m * Minimum number of days between password changes
	»» -M * Maximum number of days between password changes
	»» -W * Days of warning before password change
- - - -
# Create, Delete and Modify Groups
* `groupadd` * Add a group
	»» -g * Group ID
	»» -r * Create system group
* `groupmod` * Modify group
	»» -g * New group ID
	»» -n * New group name
* `groupdel` * Delete group
* `chmod g+s directoryname` * Set group permissions for directory, and all files created in that
directory have the same permissions

# Create, Mount, Unmount and Use VFAT, EXT4 and XFS File Systems
* **VFAT** * Extension of FAT file system, allowing log file names; often used in SAMBA shares or when sharing files between Linux and Windows computers

	»» `mkfs.ext /dev/xvdf1` * Create VFAT file system at location
	»» `mount /dev/xvdf1 /mnt/location` * Mount file system
	»» `fsck.vfat /dev/xvdf1` * Check for file system consistency

* **EXT4** * Common among Linux systems; journaling-based file system that can support up to 16TBs on Red Hat and up to 50TB in file system size
»» `mkfs.ext4 /dev/xvdf1` * Create EXT4 file system on device
»» `mount /dev/xvdf1 /mnt/location` * Mount the file system at location
»» `fsck /dev/xvdf1` * Check for file system consistency
»» `dumpe2fs /dev/xvdf1` * Get details of file system
»» `tune2fs /L labelname /dev/xvdf1` * Label the device

* `XFS` * Known for parallel processing and high I/O throughput; journaled file system that supports up to 500TB file size on Red Hat 7 with 500TB in file system size

	»» `mkfs.xfs /dev/xvdf1` * Create XFS file system on device
	»» `mount /dev/xvdf1 /mnt/location` * Mount file system at location
	»» `xfs_repair /dev/xvdf1` * Check for file system consistency
	»» `xfs_info /dev/xvdf1` * Get details of file system
	»» `xfs_admin /L labelname /dev/xdf1` * Label the device

#linux/commands