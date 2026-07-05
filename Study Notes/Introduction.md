When talking about cybersecurity, we should make sure about maintaining one thing when making a system secure - ***C.I.A***
- **Confidentiality** of non-public resources
- **Integrity** of data
- **Availability** for use

The components and their setup are very much system specific in how the component helps it and how the system is going to be used.
## Finding the lack of cybersecurity in a system
---
Instead of proving that something is secure, we try to prove there is a insecurity/vulnerability in a system.
Basically, in cybersecurity, we try to prove that the ***C.I.A.***  model does not hold for system for it to be insecure.
In `pwn.college`, the model is basically "User must not be able to manipulate the challenge into disclosing the `/flag` file's contents". We then try to disprove the ***CIA*** model by getting the contents of the `flagfile` which breaks the **Confidentiality** rule of the model. 
This proof is basically an exploit using the vulnerabilities of the system 