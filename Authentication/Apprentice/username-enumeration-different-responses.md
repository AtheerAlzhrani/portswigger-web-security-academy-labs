# Username Enumeration via Different Responses

* **Platform:** PortSwigger Web Security Academy
* **Difficulty:** Apprentice
* **Category:** Authentication – Username Enumeration
* **Status:** Solved
* **Tool:** Burp Suite

## Objective

Exploit differences in authentication responses to identify a valid username, brute-force the corresponding password, and gain access to the user's account page

## Methodology

1. Intercepted the login request using Burp Suite.
2. Identified the username and password parameters.
3. Used Burp Suite Intruder to test candidate usernames while keeping the password constant.
4. Analyzed response differences to identify a valid username.
5. Reused the identified username and tested candidate passwords using Intruder.
6. Compared server responses to identify valid credentials.
7. Submitted the discovered credentials and successfully accessed the user's account page.

## Vulnerability

The application was vulnerable to username enumeration because it returned different responses depending on whether the submitted username existed.

Additionally, insufficient brute-force protection allowed repeated password attempts, enabling an attacker to discover valid credentials.

## Impact

An attacker could enumerate valid usernames and use this information to conduct targeted password attacks, increasing the risk of unauthorized account access.

## Remediation

* Use generic authentication error messages for invalid usernames and passwords.
* Apply rate limiting to login attempts.
* Implement account monitoring and suspicious-login detection.
* Use multi-factor authentication.
* Enforce strong password policies.

## Evidence
<img width="1436" height="276" alt="Screenshot 2026-11-02 181157" src="https://github.com/user-attachments/assets/9ea78b2a-4a90-4bbc-92b3-3d10a8694ecb" />
<img width="1310" height="327" alt="image" src="https://github.com/user-attachments/assets/37c03b9c-1119-40ef-8700-26bad19b9a7e" />


<img width="1650" height="522" alt="image" src="https://github.com/user-attachments/assets/af235268-6a4f-41c1-b2e1-776c01ab0023" />


## Result

Successfully enumerated a valid username by identifying differences in authentication responses, brute-forced the corresponding password using Burp Suite Intruder, and accessed the user's account page.

