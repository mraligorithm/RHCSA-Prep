# Working with Time Services
- - - -
1. Log in to the lab server and determine which timezone should be used based on your location.

```bash
    [root@localhost]# tzselect 

    or 

    [root@localhost]# timedatectl list-timezones
```

2. Update the system's timezone to America/Chicago.

```bash
    [EXAMPLE]
    [root@localhost]# timedatectl set-timezone America/SanFransisco
```

3. Update the system time to match your current local time. Remember you need to turn off NTP first for this to work.

```bash
[root@localhost]# timedatectl set-ntp false
[root@localhost]# timedatectl set-time 02:00:00
```

4. Display current time and date information.

    `[root@localhost]# timedatectl`

5. Update the NTP time services to be 0.pool.ntp.org, 1.pool.ntp.org, 2.pool.ntp.org, and 3.pool.ntp.org.

```bash
    [root@localhost]# vim /etc/chrony.conf
    # Use public servers from the pool.ntp.org project.
    # Please consider joining the pool (http://www.pool.ntp.org/join.html).
    server 0.pool.ntp.org iburst
    server 1.pool.ntp.org iburst
    server 2.pool.ntp.org iburst
    server 3.pool.ntp.org iburst
```

6. Turn NTP back on and apply the changes to the chronyd service.

```bash
    [root@localhost]# timedatectl set-ntp true
    [root@localhost]# systemctl restart chronyd
```

7. Verify the new time servers are properly working with chronyd.

    `[root@localchost] chronyc sources -v`
#linux/commands