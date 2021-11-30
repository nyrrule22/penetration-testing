---
description: https://github.com/tomnomnom/assetfinder
---

# assetfinder

## Examples

```bash
assetfinder tesla.com
assetfinder --subs-only tesla.com
```

## Script

Updated version with Amass.

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
```