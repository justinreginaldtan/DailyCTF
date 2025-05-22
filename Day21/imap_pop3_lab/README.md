# HTB IMAP/POP3 Footprinting 🕒

> *Hack The Box*  
> *Date Solved: 2025-05-21*  
> *Category: Email/Network*
> *Tools Used: Bash*

## Table of Contents

- [HTB IMAP/POP3 Footprinting 🕒](#htb-imappop3-footprinting-)
  - [Table of Contents](#table-of-contents)
  - [Summary](#summary)
  - [Reconnaissance \& Enumeration](#reconnaissance--enumeration)
  - [Exploitation](#exploitation)
  - [Takeaways \& Lessons Learned](#takeaways--lessons-learned)

---

## Summary

I was to enumerate mail servers specifically IMAP and POP3, finding out service information, reading emails because of misconfigurations, as well as finding emails for admins.

---

## Reconnaissance & Enumeration

Given an IP address, my first step was to use nmap to gain intel. I specified port 110, 143, 993, and 995 because I wanted to find POP3 and IMAP services.

```bash
sudo nmap 10.129.14.128 -sC -sV -p 110,143,993,995
```

I found active services running for each port, along with information regarding the service versions, organization names, and more. I was also given a username from the previous lab, along with its password which was robin:robin, so my next step was to try this username and enumerate further.

---

## Exploitation

```bash
openssl s_client -connect 10.129.14.128:imaps
```

Connecting to the IMAP server using openssl, I logged in using standard IMAP commands. I read through the inboxes, and found a message dedicated to an admin. I was able to discover the admin email, as well as read the email contents which contained the flag.

## Takeaways & Lessons Learned

In conjunction with the SMTP module, I've become more familiar with the process of footprinting email services.
