# 🧠 Stop Drop and Roll
> *Hack The Box* 
> *Date Solved: 2025-05-11*  
> *Difficulty: Easy*
> *Category: Scripting*
---

## 🧩 Overview
The server gave a connection to connect with my terminal. We connect to a remote service. It sends words like GORGE, PHREAK, and FIRE. We translate each one before replying:

    GORGE → STOP

    PHREAK → DROP

    FIRE → ROLL
It was an easy task to automate with a python script.
---

## 🛠️ Tools & Techniques
- Bash
- Python
- Scripting

---

## 🔍 Approach
```python
prom pwn import *

# Connect to the new target
r = remote('83.136.252.123', 44244)

r.recvuntil(b'(y/n) ')
r.sendline(b'y')
r.recvuntil(b'\n')

tries = 0

while True:
    try:
        # Read the prompt line
        got = r.recvline().decode()

        # Transform the challenge words into your responses
        payload = (
            got
            .replace(", ", "-")
            .replace("GORGE", "STOP")
            .replace("PHREAK", "DROP")
            .replace("FIRE", "ROLL")
            .strip()
        )

        # Send the payload when asked
        r.sendlineafter(b'What do you do?', payload.encode())

        tries += 1
        log.info(f'{tries}: {payload}')

    except EOFError:
        # On end of stream, print the last line as the flag
        log.success(got.strip())
        r.close()
        break
```

---

## 🧠 Takeaways

It was a pretty straightforward CTF, but it did emphasize the importance of automation through scripting. Even though it was possible to simply input and go through 1-130 characters manually, the script had it done in less than a minute. This CTF also shows an example of the powerful tool that grep can be.