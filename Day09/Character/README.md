# 🧠 Characters
> *Hack The Box* 
> *Date Solved: 2025-05-09*  
> *Difficulty: Easy*
> *Category: Scripting*
---

## 🧩 Overview
The server gave a prompt over a raw TCP connection asking for a character index, then returned the corresponding character from the flag. Our goal was to extract the entire flag efficiently.

---

## 🛠️ Tools & Techniques
- Bash
- Bash Scripting
- Grep

---

## 🔍 Approach

**How did you solve it?**  
When connecting to the port through nc on my zsh terminal, I was prompted with:
 ```Which character (index) of the flag do you want? Enter an index```
 After trying different numbers I was given the part of the flag relating to the number I inputted, but there had to be a more efficient way. I first needed to discover the length of the flag, and I begun by entering numbers to guess the length. After trying intervals of 12, 24, 48, 96, I was able to figure out the length of the flag ended at 130 characters. Now was time to create a script to gather all the characters of the flag efficiently. I created a script to input numbers 1-130 through the connection, and then log the entire output into a seperate file named characters.log.

 ```bash
 seq 0 103 | nc -n -v 94.237.63.198 42424 | tee character.log
 ```

Then I would utilize grep to filter out extra text and gain only the flag characters, and I was able to retrieve the flag.

```bash
 grep Index  character.log|sed -E -e 's/^Which.*Index [0-9]+: (.+)/\1/' |tr -d '\n'
```
---

## 🧠 Takeaways

It was a pretty straightforward CTF, but it did emphasize the importance of automation through scripting. Even though it was possible to simply input and go through 1-130 characters manually, the script had it done in less than a minute. This CTF also shows an example of the powerful tool that grep can be.