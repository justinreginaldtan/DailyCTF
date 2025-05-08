# File-Upload-Vulnerability 🕒

## 🟢 Overview
- **Category:** Web
- **Challenge Name:** Jailbreak
- **Tools Used:** Browser, BurpSuite

## 🔍 Initial Recon
After browsing the website and logging into my given account, I found a section where I could upload an img for my avatar. I checked the GET request through Burp Suite, and was able to see I could exploit the database. It accepts files only of jpeg/image, but that could easily be tricked. I created my .php script to make the system call for passwords in the database. After uploading this .php file disguised as a .jpg file, I called for it again in a POST request.

## Impact
Any user with upload rights can run arbitrary commands on the application server. Attackers can read or modify all database contents and pivot to internal hosts.

## Root cause
The server trusts the client provided Content-Type and only validates the literal extension, then stores the file inside a directory mapped for PHP execution.
