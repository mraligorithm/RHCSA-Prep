# Boot Process
- - - -
![](Boot-Process/Screen%20Shot%202018-04-30%20at%2011.20.16%20AM.png)

1. **BIOS**
BIOS stands for Basic Input/Output System
Performs some system integrity checks
Searches, loads, and executes the boot loader program.

It looks for boot loader in floppy, cd-rom, or hard drive. You can press a key (typically F12 of F2, but it depends on your system) during the BIOS startup to change the boot sequence.

Once the boot loader program is detected and loaded into the memory, BIOS gives the control to it.

So, in simple terms BIOS loads and executes the MBR boot loader.

2. **MBR**

MBR stands for Master Boot Record.
It is located in the 1st sector of the bootable disk. Typically _dev_hda, or _dev_sda
MBR is less than 512 bytes in size. This has three components 1) primary boot loader info in 1st 446 bytes 2) partition table info in next 64 bytes 3) mbr validation check in last 2 bytes.
It contains information about GRUB (or LILO in old systems).
So, in simple terms MBR loads and executes the GRUB boot loader.

3. **GRUB**

GRUB stands for Grand Unified Bootloader.
If you have multiple kernel images installed on your system, you can choose which one to be executed.
GRUB displays a splash screen, waits for few seconds, if you don’t enter anything, it loads the default kernel image as specified in the grub configuration file.
GRUB has the knowledge of the filesystem (the older Linux loader LILO didn’t understand filesystem).
Grub configuration file is _boot_grub_grub.conf (_etc/grub.conf is a link to this). The following is sample grub.conf of CentOS.

```bash
    #boot=/dev/sda
    default=0
    timeout=5
    splashimage=(hd0,0)/boot/grub/splash.xpm.gz
    hiddenmenu
    title CentOS (2.6.18-194.el5PAE)
              root (hd0,0)
              kernel /boot/vmlinuz-2.6.18-194.el5PAE ro root=LABEL=/
              initrd /boot/initrd-2.6.18-194.el5PAE.img
```

As you notice from the above info, it contains kernel and initrd image.
So, in simple terms GRUB just loads and executes Kernel and initrd images.

4. **Kernel**

Mounts the root file system as specified in the “root=” in grub.conf
Kernel executes the _sbin_init program
Since init was the 1st program to be executed by Linux Kernel, it has the process id (PID) of 1. Do a ‘ps -ef | grep init’ and check the pid.
`initrd `stands for Initial RAM Disk.
`initrd` is used by kernel as temporary root file system until kernel is booted and the real root file system is mounted. It also contains necessary drivers compiled inside, which helps it to access the hard drive partitions, and other hardware.

5. **Init**

Looks at the `/etc/inittab` file to decide the Linux run level.

Following are the available run levels
```yaml
        0 – halt
        1 – Single user mode
        2 – Multiuser, without NFS
        3 – Full multiuser mode
        4 – unused
        5 – X11
        6 – reboot
```
Init identifies the default initlevel from _etc_inittab and uses that to load all appropriate program.
Execute ‘grep initdefault _etc_inittab’ on your system to identify the default run level

If you want to get into trouble, you can set the default run level to 0 or 6. Since you know what 0 and 6 means, probably you might not do that.
Typically you would set the default run level to either 3 or 5.

6. `Runlevel programs`

When the Linux system is booting up, you might see various services getting started. For example, it might say “starting sendmail …. OK”. Those are the runlevel programs, executed from the run level directory as defined by your run level.
Depending on your default init level setting, the system will execute the programs from one of the following directories.
```yaml
        Run level 0 – /etc/rc.d/rc0.d/
        Run level 1 – /etc/rc.d/rc1.d/
        Run level 2 – /etc/rc.d/rc2.d/
        Run level 3 – /etc/rc.d/rc3.d/
        Run level 4 – /etc/rc.d/rc4.d/
        Run level 5 – /etc/rc.d/rc5.d/
        Run level 6 – /etc/rc.d/rc6.d/
```
Please note that there are also symbolic links available for these directory under _etc directly. So, /etc_rc0.d is linked to _etc_rc.d/rc0.d.

Under the _etc_rc.d_rc*.d_ directories, you would see programs that start with S and K.
Programs starts with S are used during startup. S for startup.
Programs starts with K are used during shutdown. K for kill.
There are numbers right next to S and K in the program names. Those are the sequence number in which the programs should be started or killed.

For example, S12syslog is to start the syslog deamon, which has the sequence number of 12. S80sendmail is to start the sendmail daemon, which has the sequence number of 80. So, syslog program will be started before sendmail.

- - - -

# Boot Into Different Targets Manually
* A target is a Systemd unit of configuration that defines a grouping of services and configuration
files the must be started when the system moves into the defined target.
	»» A grouping of dependencies starts when a target is called
* `systemctl list-units —type=target` * View all targets on system
* `systemctl list-units —type=target —all` * View all targets on disk
* Common targets:
	»» `emergency.target` * su login; mounts only the root filesystem, which is read-only
	»» `multi-user.target` * Support concurrent log ins of multiple users
	»» `rescue.target` * su login; basic Systemd init
	»» `graphical.target` * Support concurrent log ins of multiple users on a graphical interface
* `systemctl get-default` * Show default target
* `systemctl set-default` * Set default target
* Configuration files:
	»» _usr_lib_systemd_system
	»» _etc_systemd/system
* `systemctl -t help` * View unit configuration types
* `systemctl status service` * Find status of service
* `systemctl —type=service` * List configuration files of active services
* `systemctl enable service` * Enable service configuration to start at boot
* `systemctl —failed` * List failed services
* Select a different target at boot:
	»» Reboot system
	»» At Grub menu, press E to edit entry
	»» Go to linux16 kernel and press CTRL+E
	»» Add systemd.unit=target.target
	»» CTRL+X
- - - -
# Interrupt Boot Process to Access System
* Start or reboot system
* Stop Grub autoselection
* Ensure the appropriate kernel is highlighted and press E to edit
* Navigate to the linux16 line, press E
* Add line rd.break
* CTRL+X
* System boots into emergency mode
* Mount `/sysroot` with read and write permissions
	»» `mount -oremount, rw /sysroot`
* Switch into chroot jail:
	»» `chroot /sysroot`
* Reset root password
* Clean up
	»» `touch /.autorelabel`
* exit
* exit
- - - -

#linux/commands