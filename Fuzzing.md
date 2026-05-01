# Commands 
# 🔍 Fuzzing

> Directory and file enumeration techniques for web applications.

---

## Gobuster

```bash
# Basic directory fuzzing
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://[IP] -n -t 200

# With file extensions
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://[IP] -n -t 200 -x .php,.html,.txt,.bak

# With authentication
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://[IP] -t 100 -x .php -U admin -P password

# HTTPS target
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u https://[IP] -k -t 150 -x .php,.js

# Subdomain fuzzing
gobuster dns -d [DOMAIN] -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -t 50

# With custom headers
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://[IP] -H "Authorization: Bearer [TOKEN]" -t 100
```

---

## Wfuzz

```bash
# Basic fuzzing
sudo wfuzz -c --hc=404 -t 500 -L -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt http://[IP]/FUZZ

# With extension
sudo wfuzz -c --hc=404 -t 200 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt http://[IP]/FUZZ.php

# Filter by response size
sudo wfuzz -c --hc=404 --hl=0 -t 200 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt http://[IP]/FUZZ

# Fuzz parameters
sudo wfuzz -c --hc=404 -t 200 -w /usr/share/wordlists/SecLists/Discovery/Web-Content/burp-parameter-names.txt http://[IP]/index.php?FUZZ=test

# POST fuzzing
sudo wfuzz -c --hc=404 -t 100 -w /usr/share/wordlists/rockyou.txt -d "username=admin&password=FUZZ" http://[IP]/login.php

# Multiple wordlists
sudo wfuzz -c --hc=404 -t 200 -w /usr/share/wordlists/users.txt -w /usr/share/wordlists/rockyou.txt http://[IP]/FUZZ/FUZ2Z
```

---

## Dirb

```bash
# Basic scan
dirb http://[IP] /usr/share/wordlists/dirb/common.txt

# With extension
dirb http://[IP] /usr/share/wordlists/dirb/common.txt -X .php,.html,.txt

# Ignore response code
dirb http://[IP] /usr/share/wordlists/dirb/common.txt -N 404

# With cookie
dirb http://[IP] /usr/share/wordlists/dirb/common.txt -c "PHPSESSID=[TOKEN]"

# HTTPS
dirb https://[IP] /usr/share/wordlists/dirb/common.txt -S

# Custom user agent
dirb http://[IP] /usr/share/wordlists/dirb/common.txt -a "Mozilla/5.0"

# Output to file
dirb http://[IP] /usr/share/wordlists/dirb/big.txt -o output.txt
```

---

## Ffuf

```bash
# Basic
ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://[IP]/FUZZ

# With extension
ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://[IP]/FUZZ -e .php,.html,.txt

# Filter size
ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://[IP]/FUZZ -fs 0

# Subdomain
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -u http://[DOMAIN] -H "Host: FUZZ.[DOMAIN]" -fs 0

# POST parameter
ffuf -w /usr/share/wordlists/rockyou.txt -u http://[IP]/login -X POST -d "user=admin&pass=FUZZ" -H "Content-Type: application/x-www-form-urlencoded" -fc 401
```
