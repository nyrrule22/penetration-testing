# ffuf

## Wordlists

```bash
# Words
/usr/share/seclists/Discovery/Web-Content/big.txt
/usr/share/seclists/Discovery/Web-Content/raft-medium-words-lowercase.txt
# Files with Extensions
/usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt
# Extensions
/usr/share/seclists/Discovery/Web-Content/web-extensions.txt
# Directories
/usr/share/seclists/Discovery/Web-Content/raft-medium-directories-lowercase.txt
# Subdomain
/usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```

## Basic Usages

```bash
# Help Page
ffuf -h
# Using FUZZ Keyword
ffuf -u http://10.10.140.239/FUZZ -w /usr/share/seclists/Discovery/Web-Content/big.txt
# Using custom Keyword
ffuf -u http://10.10.140.239/NORAJ -w /usr/share/seclists/Discovery/Web-Content/big.txt:NORAJ
```

## Finding Pages and Directories

```bash
# Checking the index page for technology
ffuf -u http://10.10.140.239/indexFUZZ -w /usr/share/seclists/Discovery/Web-Content/web-extensions.txt
# After identifying technology + basic extensions
ffuf -u http://10.10.140.239/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-words-lowercase.txt -e php,txt
# Checking lowercase pages
ffuf -u http://10.10.140.239/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories-lowercase.txt
```

## Using Filters

```bash
# Filter out HTTP Status Code: -fc 403
ffuf -u http://10.10.140.239/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt -fc 403
# Match HTTP Status Code: -mc 200
ffuf -u http://10.10.140.239/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt -mc 200
# Filter out file size: -fs 0
ffuf -u http://10.10.140.239/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt -fs 0
# Regex mathing all files that start with a .
ffuf -u http://10.10.140.239/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-files-lowercase.txt -fr '/\..*'
```

## Fuzzing Parameters

```bash
# Parameter fuzz for an API pareamter
ffuf -u 'http://10.10.140.186/sqli-labs/Less-1/?FUZZ=1' -c -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt -fw 39
ffuf -u 'http://10.10.140.186/sqli-labs/Less-1/?FUZZ=1' -c -w /usr/share/seclists/Discovery/Web-Content/raft-medium-words-lowercase.txt -fw 39
# Scripts to brute force the parameter value of a number
ruby -e '(0..255).each{|i| puts i}' | ffuf -u 'http://10.10.140.186/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 33
ruby -e 'puts (0..255).to_a' | ffuf -u 'http://10.10.140.186/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 33
for i in {0..255}; do echo $i; done | ffuf -u 'http://10.10.140.186/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 33
seq 0 255 | ffuf -u 'http://10.10.140.186/sqli-labs/Less-1/?id=FUZZ' -c -w - -fw 33
# wordlist-based brute-force attacks
ffuf -u http://10.10.140.186/sqli-labs/Less-11/ -c -w /usr/share/seclists/Passwords/Leaked-Databases/hak5.txt -X POST -d 'uname=Dummy&passwd=FUZZ&submit=Submit' -fs 1435 -H 'Content-Type: application/x-www-form-urlencoded' 
```

## Finding vhosts and Subdomains

```bash
# Resolving Public subdomain
ffuf -u http://FUZZ.mydomain.com -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
# Resolving Private subdomain
ffuf -u http://mydomain.com -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -H 'Host: FUZZ.mydomain.com' -fs 0
```

## Proxifying ffuf Traffic

```bash
# Send traffic through a web proxy
ffuf -u http://10.10.140.186/ -c -w /usr/share/seclists/Discovery/Web-Content/common.txt -x http://127.0.0.1:8080
# Send only mathces to your proxy
ffuf -u http://10.10.140.186/ -c -w /usr/share/seclists/Discovery/Web-Content/common.txt -replay-proxy http://127.0.0.1:8080
```
