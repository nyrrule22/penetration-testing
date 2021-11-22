# netcat

## Chat Line

#### From Machine 1

```bash
nc -lp 4545
```

#### From Machine 2

```bash
nc <M1 IP> 4545
```

## Copy File

Example copying a file from Machine 1 to Machine 2.

#### From Machine 2

```bash
nc -lp 4545 > incoming.txt
```

#### From Machine 1

```bash
nc <M2 IP> 4545 < myfile.txt
```

## Client for Service

### Web Server

```bash
nc -v google.com 80
GET index.html HTTP/1.1  # Then press enter twice
```

### FTP Server

```bash
nc -v <IP> 21
user anonymous
pass <password>
help
```
