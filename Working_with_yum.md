# Working_with_yum 
- - - -


1. Updating your system is an important part of being a systems administrator. However, sometimes just blindly performing a "yum update" on your system to update all packages is ill-advised. Issue the proper command to view all packages that have an available update, but do not update all packages.

    `[root@localhost ~]# yum check-update`

2. Search the yum repository for the Apache web server.

    `[root@localhost ~]# yum search apache http server`

3. View information about the Apache web server package.

    `[root@localhost ~]# yum info httpd`

4. Download and install the Apache web server.

    `[root@localhost ~]# yum install httpd`

5. List the installed packages and verify the Apache web server is in fact installed.

    `[root@localhost ~]# yum list installed httpd`

    or

    `[root@localhost ~]# yum list installed | grep httpd`

6. Issue the proper command to show all packages that provide the _var_www/html directory.

    `[root@localhost ~]# yum provides /var/www`

    or

    `[root@localhost ~]# yum whatprovides /var/www`

7. Issue the command to update the Apache web server package.

    `[root@localhost ~]# yum update httpd`

8. Remove the Apache web server package. 

    `[root@localhost ~]# yum remove httpd`
- - - -
# WORKING WITH YUM GROUPS
1. List available groups.

    `[root@localhost ~]# yum group list`

2. List all packages that belong to the "Security Tools" group. When looking at this, what packages will be installed?

    `[root@localhost ~]# yum group info "Security Tools"`

3. Install the "Security Tools" group.

    `[root@localhost ~]# yum group install "Security Tools"`

4. Undo the install of the "Security Tools" group.

```bash
    [root@localhost ~]# yum history
    Loaded plugins: amazon-id, rhui-lb
    ID     | Login user               | Date and time    | Action(s)      | Altered
    -------------------------------------------------------------------------------
        17 |                    | 2015-05-02 11:10 | Install        |    3   
        16 |                    | 2015-05-02 10:55 | Erase          |    1   
        15 |                    | 2015-05-02 10:51 | Install        |    5   

    [root@localhost ~]# yum history info 17
    Loaded plugins: amazon-id, rhui-lb
    Transaction ID : 17
    Begin time     : Sat May  2 11:10:49 2015
    Begin rpmdb    : 666:2f8b0d9de8e03b35809bb3623696ba61ab589deb
    End time       :            11:10:51 2015 (2 seconds)
    End rpmdb      : 669:84e7861867838346e3c843a204e1ad2b5a0cc4ba
    User           :  
    Return-Code    : Success
    Command Line   : group install Security Tools
    [root@localhost ~]# yum history undo 17
```




#linux/commands