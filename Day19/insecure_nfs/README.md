# [Challenge Name] DNS Footprinting 🕒
> *Hack The Box*  
> *Date Solved: 2025-05-19*  
> *Category: Networking*
> *Tools Used: NFS, mount, nmap*

## Table of Contents
- [Summary](#summary)
- [Reconnaissance & Enumeration](#reconnaissance--enumeration)
- [Exploitation](#exploitation)
- [Takeaways & Lessons Learned](#takeaways--lessons-learned)

---

## Summary
We discovered an unauthenticated NFS export named nfsshare, mounted it with nolock, and pulled down the flag.txt inside—revealing our flag.

---

## Reconnaissance & Enumeration
    Lab DNS server: 10.129.160.254 (internal authoritative server)

    Tools: dig, dnsenum, host, nmap

3. Methodology

    Zone Transfer (AXFR)

dig axfr inlanefreight.htb @10.129.160.254

Successfully retrieved all forward records for the main zone.

Sub-zone Enumeration
Identified internal.inlanefreight.htb as a delegated sub‐zone. Performed:

    dig axfr internal.inlanefreight.htb @10.129.160.254

    to pull its complete record set.

    Brute-force & Reverse Lookups
    When AXFR was refused on other sub-zones, used wordlist-based brute forcing (gobuster dns, dnsenum) and reverse PTR sweeps to find hidden hosts.

4. Findings
Hostname	Record Type	IP Address	Notes
inlanefreight.htb	SOA/NS/TXT	—	Main zone SOA & TXT (SPF, MS verification)
Delegated Sub-zone			
internal.inlanefreight.htb	A	10.129.1.6	Further AXFR allowed → see below
Primary Hosts			
app.inlanefreight.htb	A	10.129.18.15	Web application candidate
dev.inlanefreight.htb	A	10.12.0.1	Isolated “dev” network (10.12.0.0/24)
mail1.inlanefreight.htb	A	10.129.18.201	SMTP / mail infrastructure
ns.inlanefreight.htb	A	127.0.0.1	DNS server loopback (likely internal tests)

Internal Sub-zone Records (internal.inlanefreight.htb)
Hostname	A Record
dc1.internal.inlanefreight.htb	10.129.34.16
dc2.internal.inlanefreight.htb	10.129.34.11
mail1.internal.inlanefreight.htb	10.129.18.200
ns.internal.inlanefreight.htb	127.0.0.1
vpn.internal.inlanefreight.htb	10.129.1.6
ws1.internal.inlanefreight.htb	10.129.1.34
ws2.internal.inlanefreight.htb	10.129.1.35
wsus.internal.inlanefreight.htb	10.129.18.2
Hidden Flag	TXT
v=… “HTB{redacted}”

5. Analysis & Next Steps

    Hosts & Networks

        10.129.18.0/24: hosts app, mail1, wsus – focus on web and mail services.

        10.129.1.0/24: internal hosts (dc1, dc2, vpn, ws1, ws2) – potential DC services, VPN endpoints.

        10.129.34.0/24: DC subnet – target LDAP, SMB, RDP.

        10.12.0.0/24: development network (dev) – treat as separate trust zone.

    Service Enumeration

        Run a full TCP‐service scan (nmap -Pn -sV -p-) against each discovered IP.

        Prioritize: HTTP/HTTPS on app & dev; SMTP on mail1; LDAP/SMB/RDP on DCs; VPN port on vpn.

    Web Application Testing

        “app” and “dev” likely host web apps – perform directory brute forcing, login-page enumeration, and common vulnerability checks (SQLi, XSS).

    Authentication & Pivoting

        Extract credentials from TXT/SPF records (mailguns, Google, Atlassian hints) and test against web logins or internal services.

        Check for default or reused credentials on internal hosts.

    Further DNS Exploration

        Confirm no other delegated zones exist by inspecting parent .htb zone NS records.

        If AXFR is blocked elsewhere, augment with targeted brute force and reverse lookups.

6. Conclusion

Full AXFR access to both the main and internal zones yielded complete host inventories and even the capture-the-flag token. Next, conduct service scans and focused exploitation on identified targets, starting with web applications (app, dev), mail infrastructure (mail1), and internal Windows services (domain controllers, VPN, WSUS).

---

## Takeaways & Lessons Learned
I had to learn alot about DNS from how DNS servers communicated, to what the logs and information I was receiving was telling me. Using the tools wasn't foreign to me so once I learned that then extracting data become easier.