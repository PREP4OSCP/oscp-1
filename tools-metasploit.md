# Metasploit
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
