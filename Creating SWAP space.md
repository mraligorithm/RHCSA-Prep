# Creating SWAP space 
- - - -

1. Create and start the virtual machine and make sure to mount a disk.

2. The VM already has a swap enabled, so first run swapoff to disable the _swapfile in the /etc_fstab file.

    `[root@localhost etc]# swapoff -a`

    Note: swapoff -a disables the swap space listed in _etc_fstab.

3. Remove the entry _swapfile from the /etc_fstab file.

```yaml
    [root@localhost] vim /etc/fstab
    #
    # /etc/fstab
    # Created by anaconda on Fri Oct 17 18:33:48 2014
    #
    # Accessible filesystems, by reference, are maintained under '/dev/disk'
    # See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
    #
    UUID=668dbd02-c201-44bc-be76-f606fc9ab8db /                       xfs     defaults        1 1
```

4. Verify there is no current swap space available

```bash
    [root@localhost etc]# free -m
                 total       used       free     shared    buffers     cached
    Mem:           992        225        767         12          1         82
    -/+ buffers/cache:        141        851
    Swap:            0          0          0
```

5. Using fdisk, create a partition that uses the entire _dev_xvdf disk; make sure to label the device correctly.

```bash
    [root@localhost dev]# fdisk xvdf
    Welcome to fdisk (util-linux 2.23.2).

    Changes will remain in memory only, until you decide to write them.
    Be careful before using the write command.
     
    Device does not contain a recognized partition table
    Building a new DOS disklabel with disk identifier 0x69baf224.
     
    Command (m for help): n
    Partition type:
       p   primary (0 primary, 0 extended, 4 free)
       e   extended
    Select (default p): 
    Using default response p
    Partition number (1-4, default 1): 
    First sector (2048-2097151, default 2048): 
    Using default value 2048
    Last sector, +sectors or +size{K,M,G} (2048-2097151, default 2097151): 
    Using default value 2097151
    Partition 1 of type Linux and of size 1023 MiB is set
     
    Command (m for help): t
    Selected partition 1
    Hex code (type L to list all codes): 82
    Changed type of partition 'Linux' to 'Linux swap / Solaris'
     
    Command (m for help): w
    The partition table has been altered!
     
    Calling ioctl() to re-read partition table.
    Syncing disks.
```

6. Format the device with the swap signature.

    `[root@localhost dev]# mkswap /dev/xvdf1`
    `Setting up swapspace version 1, size = 1047548 KiB`	
    	`no label, UUID=5713eae2-de6b-4485-af5d-1c659765cd92`

7. Activate the swap space.

```bash
    [root@localhost dev]# swapon /dev/xvdf1
    [root@localhost dev]# free -m
                 total       used       free     shared    buffers     cached
    Mem:           992        216        776         12          0         79
    -/+ buffers/cache:        135        857
    Swap:         3046          0       3046
```

8. Add swap space to the _etc_fstab so that it is a persistent mount.

```bash
    [root@localhost dev]# blkid
    /dev/xvda2: UUID="" TYPE="xfs" PARTUUID="9146b810-9a31-4c10-a206-01b0bbaca807" 
    /dev/xvdf1: UUID="" TYPE="swap" 
    Grab the UUID to mount the swap device.
    [root@localhost dev]# vim /etc/fstab
    #
    # /etc/fstab
    #
    UUID=668dbd02-c201-44bc-be76-f606fc9ab8db /                       xfs     defaults        1 1
    UUID=YOUR-UUID swap  swap    defaults 0 0 
```

9. Activate the swap space that is added in the _etc_fstab file.

    Since we activated it manually already, this manually deactivates it, then activates it based on the _etc_fstab file.

    `[root@localhost dev]# swapoff /dev/xvdf1`

    Now activate it persistently based off the _etc_fstab entry:

    `[root@localhost dev]# swapon -a`

    -a activates all swap spaces located in the _etc_fstab.
- - - -

#linux/commands