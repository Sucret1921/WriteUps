# 🌐 Network Recon

> Network scanning, enumeration and discovery techniques.

---

## Nmap

```bash
# Host discovery — no port scan
nmap -sn [IP]/24

# Fast scan — top 100 ports
nmap -F [IP]

# Full port scan
nmap -p- [IP]

# Service and version detection
nmap -sV [IP]

# OS detection
nmap -O [IP]

# Aggressive scan (OS + version + scripts + traceroute)
nmap -A [IP]

# Stealth SYN scan
nmap -sS [IP]

# UDP scan
nmap -sU [IP]

# Specific ports
nmap -p 22,80,443,3306,8080 [IP]

# Port range
nmap -p 1-10000 [IP]

# Skip ping — treat host as online
nmap -Pn [IP]

# No DNS resolution
nmap -n [IP]

# Combined recon
nmap -sV -sC -p- -n -Pn --open [IP]

# Script scan — default scripts
nmap -sC [IP]

# Specific script
nmap --script=http-enum [IP]
nmap --script=smb-vuln* [IP]
nmap --script=ftp-anon [IP]
nmap --script=ssh-brute [IP]

# Vulnerability scan
nmap --script vuln [IP]

# Output to file
nmap -sV -oN output.txt [IP]
nmap -sV -oX output.xml [IP]
nmap -sV -oA output [IP]

# Scan subnet
nmap -sn 192.168.1.0/24

# Timing templates (T0 paranoid → T5 insane)
nmap -T4 -p- [IP]

# Firewall evasion — fragment packets
nmap -f [IP]

# Decoy scan
nmap -D RND:10 [IP]

# Scan from list
nmap -iL targets.txt
```

---

## Netcat

```bash
# Banner grabbing
nc -nv [IP] [PORT]

# Listen on port
nc -lvnp [PORT]

# Reverse shell listener
nc -lvnp 4444

# Port scan
nc -zv [IP] 1-1000

# File transfer — sender
nc -lvnp 4444 < file.txt

# File transfer — receiver
nc [IP] 4444 > file.txt
```

---

## Enum Network

```bash
# ARP scan — discover hosts
arp-scan -l
arp-scan [IP]/24

# Ping sweep
for i in $(seq 1 254); do ping -c 1 192.168.1.$i | grep "bytes from"; done

# Traceroute
traceroute [IP]

# DNS lookup
nslookup [DOMAIN]
dig [DOMAIN]
dig [DOMAIN] ANY
dig axfr [DOMAIN] @[DNS-SERVER]

# Whois
whois [DOMAIN]

# Check open ports locally
ss -tulnp
netstat -tulnp

# Route table
ip route
route -n
```

---

## SMB Enumeration

```bash
# Enum shares
smbclient -L //[IP] -N
smbclient -L //[IP] -U admin

# Connect to share
smbclient //[IP]/[SHARE] -N

# Enum with nmap
nmap --script smb-enum-shares,smb-enum-users [IP]

# crackmapexec
crackmapexec smb [IP]
crackmapexec smb [IP] -u '' -p '' --shares
```

---

## FTP Enumeration

```bash
# Anonymous login
ftp [IP]
# user: anonymous  pass: (empty)

# Nmap FTP scripts
nmap --script ftp-anon,ftp-bounce,ftp-syst [IP] -p 21
```
