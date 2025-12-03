
# Fuzzing 

sudo gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://[IP] -n -t 200 -x .php

sudo wfuzz -c --hc=404 -t 500 -L -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt http://[IP]/FUZZ

# Network 

nmap -P -nP [IP] 

