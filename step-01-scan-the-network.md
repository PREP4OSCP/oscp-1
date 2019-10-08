# STEP 01 - SCAN THE NETWORK

1. ping-sweep the network  
```
nmap -v -sn 10.11.1.1-254 -oG ping-sweep.txt
```

2. filter for `live` hosts  
``` 
grep Up ping-sweep.txt | cut -d " " -f 2 > live-hosts.txt
```

2.1 search the network for port #53 (dns server)
```
nmap -p 53 10.11.1.1-255 -oG dns-sweep.txt
cat dns-sweep.txt | grep open
```
copy the dns server address and add it to resolv.conf as the first nameserver `/etc/resolv.conf`  
also the following file `/etc/network/interfaces`
```
dns-nameservers 10.11.1.220 8.8.8.8
```

2. get all hostnames
> tested - cli
```
for ip in $(seq 1 254);do nslookup 10.11.1.$ip;done > nslookup.txt
cat nslookup.txt | grep name > hostnames.txt
```
