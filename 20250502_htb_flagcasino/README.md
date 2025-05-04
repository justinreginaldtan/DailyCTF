# Casino (Reversing write‑up)

The team stumbles into an abandoned casino run by creepy robots. They offer you riches if you can beat the game—so naturally, we reverse the game instead.

---

## Challenge files

| File     | Notes                          |
|----------|--------------------------------|
| `casino` | 64-bit ELF binary, not stripped |

---

## Quick recon

```bash
file casino
# → ELF 64-bit LSB pie executable, x86-64

strings -n 6 casino
# → [ ** WELCOME TO ROBO CASINO ** ]
# → [ * CORRECT * ] / [ * INCORRECT * ]
