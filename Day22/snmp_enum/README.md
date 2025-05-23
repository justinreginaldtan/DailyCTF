# SNMP Enumeration Write-Up 🕒
>
> *Hack The Box*  
> *Date Solved: 2025-05-22*  
> *Category: SNMP/Networking*
> *Tools Used: braa, onesixtyone*

## Table of Contents

- [SNMP Enumeration Write-Up 🕒](#snmp-enumeration-write-up-)
  - [Table of Contents](#table-of-contents)
  - [Summary](#summary)
  - [Reconnaissance \& Enumeration](#reconnaissance--enumeration)
  - [Takeaways \& Lessons Learned](#takeaways--lessons-learned)

---

## Summary

Brute-force the SNMP community string on 10.129.14.128 then identify the custom agent version.

---

## Reconnaissance & Enumeration

Commands

```bash
# 1. Find community string
onesixtyone -c /usr/share/seclists/Discovery/SNMP/snmp.txt 10.129.14.128

# 2. Probe system group for metadata
braa public@10.129.14.128:.1.3.6.*
```

Output

```bash
# onesixtyone
10.129.14.128 [public] Linux htb 5.11.0-37-generic …

# braa
…:sysName   : htb
…:sysLocation: InFreight SNMP v0.91

```

---
It shows the default public community string is valid, and revealed the custom SNMP agent version for me. These were what was required in the lab.

## Takeaways & Lessons Learned

Learned more networking fundamentals through this lab. It was important to understand how SNMP works, what OID and MIB tells about a system.
