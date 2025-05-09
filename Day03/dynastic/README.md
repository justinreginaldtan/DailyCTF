# 🧠 Dynastic Writeup
> *Hack The Box*  
> *Date Solved: 2025-05-03*  
> *Difficulty: Easy*
> *Category: Encryption*

A quick and friendly walkthrough of how I cracked the “Dynastic” puzzle from Hack The Box.

---

## Scenario

You wake up in a dark gas chamber.  
Handcuffed.  
Door locked.  
Tape says hydrogen cyanide will flood the room in fifteen minutes.  
A half‑dead torch lights up bloody letters on the wall plus a sketch of a Roman emperor.  
We also get two files:

* **source.py** – Python that encrypts a secret string  
* **output.txt** – the encrypted string and a note telling us to wrap our answer with `HTB{ }`


---

## Recon

Open **source.py** and look at the key loop:

```python
for i in range(len(msg)):
    ch = msg[i]
    if ch.isalpha():
        chi = ord(ch) - 65          # A → 0
        ech = chr((chi + i) % 26 + 65)

Each letter is shifted forward by its own index.
That screams “Trithemius cipher” (a rolling Caesar).
```

## Decrypt script

We just subtract the index instead of adding it.
```
cipher = open("output.txt").read().strip()

plain = []
for i, ch in enumerate(cipher):
    if ch.isalpha():
        idx = (ord(ch) - 65 - i) % 26
        plain.append(chr(idx + 65))
    else:
        plain.append(ch)

print("".join(plain))
```
