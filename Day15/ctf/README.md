# Locked Away
**Date**: 2025-05-15
**Category**: Pwn/Misc

## Table of Contents
- [Summary](#summary)
- [Reconnaissance & Enumeration](#reconnaissance--enumeration)
- [Exploitation](#exploitation)
- [Takeaways & Lessons Learned](#takeaways--lessons-learned)

---

## Summary
Given a toy python guarded by a hardcoded blacklist. I connect and see:
```bash
The chest lies waiting...
```
I have to find a way to bypass the filter and execute code to print the flag.

---

## Reconnaissance & Enumeration
The first step I did was go over the .py source code provided. I immediately noticed the command to run the flag which was open_chest():
but following was a list of blacklisted words that would exit the program if used. I thought of many different ways to bypass this blacklist that contained the common bypassers such as exec, eval, and open which was in open_chest.

My first idea was to find a way to input the blacklist letters in another form, maybe constructing each word with .join and chr(x). However, I reminded myself that there's usually and easier way and rerouted my train of thought to the basics of Python. There is a python function .clear(), which could maybe clear the blacklist and allow free input. So that would be my priority in testing.

---

## Exploitation
I connected to the remote Python REPL and was given the expected prompt. I inputted the following with no errors.
```bash
blacklist.clear()
```
To see if it actually worked, I tested open_chest():
I was then given the flag!
## Takeaways & Lessons Learned
- Blacklists and many other limiters can be bypassed and wiped out with a simple command if not properly defended against.
- Once again recon and enumeration is shown to be the most important phase in the pentest cycle.