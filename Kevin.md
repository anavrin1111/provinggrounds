# Kevin

## Info

Kevin is an easy rated windows machine from Offsec proving grounds.

## Nmap

All tcp ports.

```
80/tcp    open  http         syn-ack ttl 125 GoAhead WebServer
135/tcp   open  msrpc        syn-ack ttl 125 Microsoft Windows RPC
139/tcp   open  netbios-ssn  syn-ack ttl 125 Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds syn-ack ttl 125 Windows 7 Ultimate N 7600 microsoft-ds (workgroup: WORKGROUP)
3389/tcp  open  tcpwrapped   syn-ack ttl 125
3573/tcp  open  tag-ups-1?   syn-ack ttl 125
49152/tcp open  msrpc        syn-ack ttl 125 Microsoft Windows RPC
49153/tcp open  msrpc        syn-ack ttl 125 Microsoft Windows RPC
49154/tcp open  msrpc        syn-ack ttl 125 Microsoft Windows RPC
49155/tcp open  msrpc        syn-ack ttl 125 Microsoft Windows RPC
49158/tcp open  msrpc        syn-ack ttl 125 Microsoft Windows RPC
49159/tcp open  msrpc        syn-ack ttl 125 Microsoft Windows RPC
```

## HTTP 80

The following credentials allow login.

```
admin:admin
```

## Root

HP Power Manager 4.2 (build 7) is vulnerable to this metasploit module.

```
windows/http/hp_power_manager_filename
```

This application was mis-configured to run as nt authority\system.
