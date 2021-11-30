---
description: https://github.com/OWASP/Amass
---

# Amass

## Examples

```bash
amass enum -d tesla.com
```

## Script

Combined with assetfinder script. Updated version with Httprobe.

```bash
#!/bin/bash

url=$1

if [ ! -d "$url" ]; then
    mkdir $url
fi
if [ ! -d "$url/recon" ];then
    mkdir $url/recon

echo "[+] Harvesting subdomains with assetfinder..."
assetfinder $url >> $url/recon/assets.txt
cat $url/recon/assets.txt | grep $1 >> $url/recon/final.txt
rm $url/recon/assets.txt

echo "[+] Harvesting subdomains with Amass..."
amass enum -d $url >> $url/recon/f.txt
sort -u $url/recond/f.txt >> $url/recon/final.txt
rm $url/recond/f.xt
```
