# 2FA Simple Bypass

**Platform:** PortSwigger Web Security Academy
**Difficulty:** Apprentice
**Category:** Authentication – Multi-Factor Authentication
**Status:** Solved
**Tool:** Burp Suite, Web Browser

## Objective

Exploit improper enforcement of two-factor authentication (2FA) to access another user's account without providing the required verification code.

## Methodology

1. Logged in using the provided test account credentials (`wiener:peter`).
2. Retrieved the 2FA verification code through the application's email client.
3. Accessed the account page and identified its URL (`/my-account`).
4. Logged out of the test account.
5. Logged in using the victim's credentials (`carlos:montoya`).
6. When prompted for the 2FA verification code, manually modified the URL to navigate directly to `/my-account`.
7. Successfully accessed Carlos's account without completing the 2FA verification process.

## Exploitation

**Normal authentication flow:**

1. Submit username and password.
2. Complete the 2FA verification.
3. Access the protected account page.

**Bypass technique:**

After submitting valid credentials, the application redirected the user to the 2FA verification page.

Instead of submitting a verification code, navigating directly to the protected `/my-account` endpoint allowed access to the victim's account.

## Vulnerability

The application failed to properly enforce the 2FA verification step before granting access to protected resources.

It relied on the authentication flow without independently verifying that the second authentication factor had been completed.

## Impact

An attacker with valid username and password credentials could bypass the second authentication factor and gain unauthorized access to another user's account.

This defeats the additional security provided by 2FA.

## Remediation

* Enforce server-side 2FA verification before granting access to protected resources.
* Maintain authentication state on the server.
* Ensure protected endpoints verify that the 2FA process is complete.
* Do not rely solely on client-side redirects or URL navigation.
* Invalidate incomplete authentication sessions appropriately.

## Evidence

*Screenshot showing the 2FA verification page, direct navigation to `/my-account`, and successful access to Carlos's account.*

## Result

Successfully bypassed the 2FA verification step by directly navigating to the protected account endpoint and accessed Carlos's account without providing the verification code.
