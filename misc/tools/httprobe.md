---
description: https://github.com/tomnomnom/httprobe
---

# httprobe

## Examples

```bash
cat tesla.com/recon/final.txt | httprobe -s -p https:443 | sed 's/https\?:\/\///' | tr -d ':443'
```

## Script

Combined with assetfinder and Amass script.

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

echo "[+] Probing for alive domains..."
cat $url/recon/final.txt | sort -u | httprobe -s -p https:443 | sed 's/https\?:\/\///' | tr -d ':443' >> alive.txt

```
