# GHCTF-The-Chamber-of-Secrets

## Challenge Description

**Name:** The Chamber of Secrets

**Points:** 365

**Description:** Sometimes you need to do none, to get the mission done ;)

**Challenge link:** [The Chamber of Secrets challenge link](http://greenhat.microclub.info:5622/)

## Tools and Techniques

**Tools:** Burp Suite, JWT Decoder

**Techniques:**

- JWT manipulation
- Changing the algorithm to "none"
- Token replacement

## Solution

### Initial Approach

**Step 1:** Intercepted the request using Burp Suite and sent it to Repeater.

**Step 2:** Decoded the JWT token using [jwt.io](https://jwt.io/).

  Decoded Payload:
  ```json
  {
  "alg": "HS256",
  "typ": "JWT"
  }
  {
    "role": "guest",
    "iat": 1722000048
  }
  ```

**Step 3:** Modified the JWT algorithm to "none" and the role to "admin".

**Detailed Solution**


 - Step 1: Open Burp Suite and intercept the request containing the JWT token.

 - Step 2: In the Decoder tab, modify the JWT header to change the alg value to none. The new header should look like this:

```json
{"alg": "none", "typ": "JWT"}
```

Note: Ensure there are no spaces between the curly braces and the contents before encoding the header in Base64.

 - Step 3: Encode the modified header in Base64.

 - Step 4: Update the payload to change the role from "guest" to "admin" and encode it in Base64 same note as the previous one .

**Step 4:** Construct the new JWT token by combining the modified header and payload. The new token format should be:

   [Base64 Encoded Header].[Base64 Encoded Payload]

   New token : 

   `eyJhbGciOiAibm9uZSIsInR5cCI6ICJKV1QifQ.eyJyb2xlIjogImFkbWluIiwiaWF0IjogMTcyMjAwMDA0OH0.`

**Step 5:** Replace the old JWT token in the Repeater request with the new token and Send the modified request.

Successfully logged in as an admin and obtained the flag.

## The Flag:

### microCTF{jw7_n0n3_4lg0r17hm_5ucc355}
