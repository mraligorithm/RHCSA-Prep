# Working with MBR Partitions
- - - -

1. Set up the environment. Once completed, log in to the system and navigate to the /dev directory.

		`[root@localhost] cd /dev`

2. Create a primary Linux partition that is 500M in size on the attached disk.

    `[root@localhost] fdisk xvdf`

```yaml
    Command (m for help): n 
    Partition type:    p   primary (0 primary, 0 extended, 4 free)    e   extended Select (default p): p 
    Partition number (1-4, default 1): 
    First sector (2048-2097151, default 2048): 
    Using default value 2048
    Last sector, +sectors or +size{K,M,G} (2048-2097151, default 2097151): +500M
    Partition 1 of type Linux and of size 500 MiB is set 
    Command (m for help): 
```

3. Set the partition type for a basic Linux volume.

```yaml
    Command (m for help): t
    Selected partition 1 Hex code (type L to list all codes): 83

    Changed type of partition 'Linux' to 'Linux' (notice the default is already Linux).
```

4. Write changes and exit.

```yaml
    Command (m for help): w 
    The partition table has been altered! Calling ioctl() to re-read partition table. Syncing disks.
```

5. Issue the command to list the block device and it's UUID (Universally Unique Identifier).

    `[root@localhost] blkid`

6. Create an XFS filesystem on the disk.

    `[root@localhost] mkfs -t xfs /dev/xvdf1`

7. Mount the partition to _mnt_mymount.

```yaml
    [root@localhost] mkdir /mnt/mymount
    [root@localhost] mount /dev/xvdf1 /mnt/mymount
    [root@localhost] df -h
    Filesystem      Size  Used Avail Use% Mounted on 
    /dev/xvda2      6.0G  3.9G  2.1G  65% /
    devtmpfs        482M     0  482M   0% /dev 
    tmpfs          497M     0  497M   0% /dev/shm 
    tmpfs           497M   13M  484M   3% /run 
    tmpfs          497M     0  497M   0% /sys/fs/cgroup
    /dev/xvdf1 497M   26M  472M   6% /mnt/mymount
```

8. Configure the disk to mount to the _mnt_mymount mount point automatically during system boot.

```yaml
    [root@localhost] blkid
    /dev/xvdf1: UUID="" TYPE="xfs"
    [root@localhost] vim fstab 
    UUID="your uuid here" /mnt/mymount xfs defaults 1 1
    [root@localhost] umount /mnt/mymount
    [root@localhost] mount -a
    [root@localhost] df -h
    Filesystem Size Used Avail Use% Mounted on
    /dev/xvda2 6.0G 3.9G 2.1G 65% /
    devtmpfs 482M 0 482M 0% /dev
    tmpfs 497M 13M 484M 3% /run
    tmpfs 497M 0 497M 0% /sys/fs/cgroup
    /dev/xvdf1 497M 26M 472M 6% /mnt/mymount
```


#linux/commands