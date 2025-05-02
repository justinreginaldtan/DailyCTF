What they gave us

    source.py that scrambles a message

    output.txt with the scrambled text and a hint to wrap the answer in HTB{ }

Step 1 Look at the code

for i in range(len(msg)):
    if char is letter:
        new = (char value + i) mod 26

Each letter shifts forward by its position index.
That is the classic Trithemius cipher, which feels like a Caesar shift that changes every step.
Step 2 Reverse the shift

For every letter at position i do
plain = (cipher value − i) mod 26.

Tiny Python to do it:

cipher = open("output.txt").read().strip()
plain = []
for i, ch in enumerate(cipher):
    if ch.isalpha():
        plain_idx = (ord(ch) - 65 - i) % 26
        plain.append(chr(plain_idx + 65))
    else:
        plain.append(ch)
print("".join(plain))

Result:
            HTB{redacted}
