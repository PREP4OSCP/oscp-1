# STEP 01 - SCAN THE HOST
## Vulnerability Assesment
We'll run a `deep scan` of the host in order to find exposed services which we might be able to exploit.  
- `exploit-db.com`, a great place to start searching for existing exploits.

> port numbers can be misleading... 80 -> http?, 22 -> ssh?

1. light scan  
```
# TCP Scan
nmap 10.11.1.71 --top-ports 10 --open
nmap 10.11.1.71 --top-ports 100 --open
# UDP Scan
nmap -sUV -T4 10.11.1.71
unicornscan -mU 10.11.1.71
```

2. heavy scan  
We are doing a complete scan (TCP 1 - 65,535), by using "-p-", which is a lot more network 'heavy' (so it's going to take longer).
We are also going to start grabbing the service banners, based on the default service port, by doing "-sV".
It is possible to get nmap to show its justification for its results by doing "--reason".
```
# TCP Scan
nmap 10.11.1.71 -p- -sV --reason --dns-server 10.11.1.220
# need to test it
nmap -v -p 1-65535 -sV -O -sT 10.11.1.71
```

3. port knocking  
Quick bash loop script for the port knocking.
```
#!/bin/bash
HOST=$1
shift
for ARG in "$@"
do
    nmap -Pn --host-timeout 100 --max-retries 0 -p $ARG $HOST
done
```
Usage:  
```
./port-knocking.sh 10.11.1.71 1 2 3 4 5 && telnet 10.11.1.71 8888
./port-knocking.sh 10.11.1.71 7000 8000 9000 7000 8000 && telnet 10.11.1.71 8888
```







