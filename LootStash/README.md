# LootStash (Reversing write‑up)

Looking for one OP item in a mountain of loot? Same vibe here—the flag is tucked inside a single executable called **`stash`**.

---

## Challenge files

| File   | Notes                                  |
|--------|----------------------------------------|
| `stash` | 64‑bit Linux ELF binary (no extra hints) |

Goal: sift through the binary and pull out the flag.

---

## Recon

### 1  Identify the binary type

```bash
file stash
# → stash: ELF 64-bit LSB executable, x86-64, version 1 (SYSV) …
```

### 2  Skim with strings
```
strings -n 6 stash | head -n 20
Bronze_Dagger
Iron_Dagger
Steel_Dagger
…
Obsidian_Warhammer
HTB{n33dl3_1n_a_l00t_stack}
Mythic_Scythe
```
### 3 Open the binary in Ghidra / Cutter:

    .rodata is filled with the weapon‑name list.

    HTB{redacted} is found! yay
