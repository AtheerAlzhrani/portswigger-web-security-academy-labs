# Username Enumeration via Different Responses

* **Platform:** PortSwigger Web Security Academy
* **Difficulty:** Practitioner
* **Category:** Authentication
* **Status:** Solved
* **Tool:** Burp Suite

## Objective

Exploit differences in login responses to identify valid usernames and use password brute-force techniques to gain access to another user's account.

## Methodology

1. Intercepted the login request using Burp Suite.
2. Identified the username and password parameters.
3. Used the provided candidate username wordlist to identify a valid username by analyzing differences in server responses.
4. Identified the valid username by observing the application's response behavior.
5. Used the candidate password wordlist to identify the correct password.
6. Successfully authenticated using the discovered credentials.

## Payloads

**Username and password discovered:**

```text
username=auth&password=shadow
```

## Vulnerability

The application reveals information about valid usernames through differences in authentication responses. This allows attackers to enumerate usernames and subsequently perform password brute-force attacks.

## Impact

* Username enumeration.
* Increased risk of account compromise.
* Unauthorized access to user accounts.
* Potential exposure of sensitive user information.

## Remediation

* Return generic error messages for failed login attempts.
* Ensure authentication responses are consistent for valid and invalid usernames.
* Implement rate limiting and account lockout protections.
* Apply multi-factor authentication (MFA).
* Monitor and detect repeated failed login attempts.

## Evidence

*Add a screenshot showing successful lab completion.*
