# Phreaks
**Date**: 2025-05-14  
**Category**: Forensics

## Table of Contents
- [Summary](#summary)
- [Reconnaissance & Enumeration](#reconnaissance--enumeration)
- [Exploitation](#exploitation)
- [Takeaways & Lessons Learned](#takeaways--lessons-learned)

---

## Summary
Given a .pcap file, I was tasked with analyzing it through Wireshark and finding out the "mole" that's sending keys.

---

## Reconnaissance & Enumeration

I started by simply looking at the logs, event types, etc, to find anything that sticks out right away. I found emails being sent involving .zip files and this was a flag for me to look into further. I used parameter smtp.data.fragment in the filter to find 15 packets of emails being sent with .zip. This means that the zip is probably sent, seperated between this 15 packets. After reading through more, I found out it's encoded in base64, that the password is also found in each packet, and the block of the base64 encoding for each packet. 

---

## Exploitation
I realized I need to decode and put together each of these packets. So I started echoing the base64 string, passed it to a decoder, then outputting the decoded string to the zip file. I then used the password found earlier to save the file for later. I repeated this for all 15 packets. 

This was the basic syntax:
```bash
echo <"base64 string here"> | base64 -d > <challenge_filename.zip> unzip -P <password> <your file name.zip>
```

After completing all 15 packets, I merged them all into one pdf file and opened them. At the end of the file, the flag was there!

## Takeaways & Lessons Learned
Straightforward CTF. It's important to know the basics of Wireshark though, and it was a good refresher.
 - I was a little confused before reminding myself of the different common encodings such as Base64, which is everywhere in emails.
 - I could have also saved time by automating some of the repetitive steps. A simple bash loop to decode and concatenate fragments would have saved me time and prevent typos.
 - Reassembly matters: Knowing that attachments can span packets drove the manual merge—later you can automate via tshark -z follow or Wireshark’s Export Objects.