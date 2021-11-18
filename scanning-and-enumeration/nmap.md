# Nmap

## Basic Scans

```bash
nmap <IP>  # Basic scan of the top 1000 ports
nmap -p- <IP>  # Basic scan of all 65535 ports
```

## Host Discovery

```bash
nmap -sn <IP>  # Ping Scan - disable port scan
nmap -Pn <IP>  # Treat all hosts as online -- skip host discovery
```

## Scan Techniques

```bash
nmap -sS <IP>  # SYN scan
nmap -sU <IP>  # UDP scan
```

## Port Specification

```bash
nmap -p22 <IP>  # Scan a specific port
nmap -p22,80,443 <IP>  # Scan multiple specific ports
nmap -p1-65535 <IP>  # Scan a range of ports
```

## Service/Version & OS Detection

```bash
nmap -sV <IP>  # Probe open ports to determine service/version info
nmap -O <IP>  # Enable OS detection
nmap -A <IP>  # Enable OS detection, version detection, script scanning, and traceroute
```

## Script Scan

```bash
nmap -sC <IP>  # equivalent to --script=default
nmap --script=<script name>  # Specify a script or scripts using regex
nmap --script <filename>|<category>|<directory>/|<expression>[,...]
nmap --script-updatedb  # Update the script database
```

## Scan Speeds

```bash
nmap -T1 <IP>  # Slowest
nmap -T2 <IP>
nmap -T3 <IP>  # Default speed when not specified
nmap -T4 <IP>
nmap -T5 <IP>  # Fastest
```

## Output

```bash
nmap <IP> -oN <filename>  # Output scan in normal format
nmap <IP> -oX <filename>  # Output scan in XML format
nmap <IP> -oG <filename>  # Output scan in Grepable format
nmap <IP> -oA <filename>  # Output scan in All 3 formats
```
