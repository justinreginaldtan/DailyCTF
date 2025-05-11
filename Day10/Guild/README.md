# 🧠 Characters
> *Hack The Box* 
> *Date Solved: 2025-05-10*  
> *Difficulty: Easy*
> *Category: Web*
---

## 🧩 Overview
The server gave a 94.237.55.43:32902.

---

## 🛠️ Tools & Techniques
- Bash
- Bash Scripting
- Grep

---

## 🔍 Approach

I started my reconnaissance using whatweb, connecting through BurpSuite and building the site map and auth flow, and then enumerating through common directories with BurpSuite. My first initial suspicions I wanted to check was the file upload in case there was a vulnerability there, maybe I could upload a .php when it rerquests an image for a reverse shell. The second thing I wanted to test was if there was any inproper cookie validation requests. Maybe there would be a way for horizontal escalation or vertical due to misconfigured role checks.

After doing more recon I was able to find that users were able to write and upload text for a sharable link. This is a big clue that there may be a vulnerability related to SSTI. I tried a sample payload trying {{1-1}} and it worked! I was then able to trick the server to find the admins username and email.

Next was to find the password, so using the forgetpassword and mapping around there, I found out the email reset link is used by hashing the email's user with SHA-256. So I could replicate the link, and then change the password for the admin account. After finding where the uploads were sent, I was able to send in a payload to retrieve the flag!


