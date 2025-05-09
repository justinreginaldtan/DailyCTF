# 🧠 (Name of Challenge)
> *Hack The Box*  
> *Date Solved: 2025-05-08*  
> *Difficulty: Easy*
> *Category: Forensics*
---

## 🧩 Overview

"Our team noticed work disappearing from the development server. We were given SSH logs and bash history to identify the attacker."
My goal is to extract and read over SSH Logs and the Bash history to find the attacker.

---

## 🛠️ Tools & Techniques
- terminal
- bash and ssh logs

---

## 🔍 Approach
  
My goal was to find
1. The SSH server's IP and port
2. The first successful login
3. The time of the unusual login
4. The attacker's public key fingerprint
5. The final command executed by the attacker

It was pretty straightforward. After reading and matching timestamps to suspicious activity in the bash history with the server logs, it wasn't hard to track the questions asked. 
---

## 🧠 Takeaways

I've done forensics problems before so this was a refresher for the most part. I did recognize the importance of analyzing SSH logs for susppcious patternns like failed key attempts, root logins at odd hours, and cleanup commads like shred. It also helped reinforced the idea how attackers often leave subtle but trackable traces, even in a failed login.