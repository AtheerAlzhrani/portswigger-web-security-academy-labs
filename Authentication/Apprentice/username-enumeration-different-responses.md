# Username Enumeration via Different Responses

**Platform:** PortSwigger Web Security Academy
**Difficulty:** Apprentice
**Category:** Authentication – Username Enumeration
**Status:** Solved
**Tool:** Burp Suite

## Objective

Exploit differences in login error messages to identify valid usernames and gain access to a user account.

## Methodology

1. Accessed the application's login page.
2. Intercepted the login request using Burp Suite.
3. Tested different usernames and observed the server's responses.
4. Identified that the application returned different error messages depending on whether the username existed.
5. Used Burp Suite Intruder to automate username enumeration.
6. Identified a valid username based on the response differences.
7. Used the valid username to identify the corresponding password.
8. Successfully authenticated and solved the lab.

## Vulnerability

The application disclosed username validity through inconsistent authentication error messages.

Instead of returning a generic login failure message, the application revealed whether a submitted username existed.

## Impact

An attacker could enumerate valid usernames and use this information to conduct targeted password attacks, increasing the risk of unauthorized account access.

## Remediation

* Use generic authentication error messages for invalid usernames and passwords.
* Apply rate limiting to login attempts.
* Implement account monitoring and suspicious-login detection.
* Use multi-factor authentication.
* Avoid revealing account existence through differences in application responses.

## Evidence

*Screenshot showing the different login responses and successful lab completion.*

## Result

Successfully identified valid usernames by analyzing authentication response differences and used the discovered credentials to authenticate successfully.

