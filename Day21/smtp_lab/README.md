# Foot-printing SMTP Write-Up
>
> *Hack The Box*  
> *Date Solved: 2025-05-21*  
> *Category: Mail/SMTP*
> *Tools Used: nmap, Kali Linux*

## Table of Contents

- [Foot-printing SMTP Write-Up](#foot-printing-smtp-write-up)
  - [Table of Contents](#table-of-contents)
  - [Summary](#summary)
  - [Reconnaissance \& Enumeration](#reconnaissance--enumeration)
  - [Exploitation](#exploitation)
  - [Takeaways \& Lessons Learned](#takeaways--lessons-learned)

---

## Summary

Understanding SMTP and the process, I had to find a way to enumerate the service used and users with a given IP.

---

## Reconnaissance & Enumeration

Given an IP address, my first step was to use nmap to gain intel. I specified port 25 because I knew this is an SMTP challenge.

```bash
sudo nmap 10.129.14.128 -sC -sV -p25
```

The output of the nmap command was:

```bash
InFreight ESMTP v2.11
```

---

## Exploitation

After finding out the service, I ran scripts from NSE to find out if the SMTP server has open relay.

```bash
sudo nmap 10.129.14.128 -p25 --script smtp-open-relay -v
```

While it did, the output did not show anything relating to the user on the server I needed to find. Going through my notes I could not find a way I would do this, so I looked on google and found the smtp-user-enum which might be helpful. I used a wordlist provided, and through Kali Linux I was able to use the command, and enumerate and guess users on the server, and found out that way.

## Takeaways & Lessons Learned

This module was a refresher mostly on the process of SMTP and the process of sending mail from:
    1. MUA → MSA
    2. MSA → MTA
    3. MTA → MDA
    4. MDA → MUA
After reminding myself of this, I followed the basic enumeration process to find services asked for, and understood the output of certain commands more.
