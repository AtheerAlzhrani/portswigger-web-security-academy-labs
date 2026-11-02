# Password Reset Broken Logic

* **Platform:** PortSwigger Web Security Academy
* **Difficulty:** Apprentice
* **Category:** Authentication – Password Reset
* **Status:** Solved
* **Tool:** Burp Suite (Proxy, HTTP History, Repeater)

## Objective

Exploit a vulnerability in the password reset functionality to change another user's password without providing a valid password reset token, then access their account.

## Methodology

1. Intercepted the password reset process using Burp Suite.
2. Requested a password reset for the test account (`wiener`).
3. Retrieved the password reset email through the email client.
4. Followed the reset link and changed the test account's password.
5. Examined the password reset requests under **Proxy > HTTP history**.
6. Identified the `temp-forgot-password-token` parameter in the reset request.
7. Sent the password reset POST request to Burp Repeater.
8. Removed the reset token value from both the URL and request body.
9. Observed that the application still processed the password reset request without validating the token.
10. Modified the `username` parameter from `wiener` to `carlos` and supplied a new password.
11. Successfully changed Carlos's password and logged in to his account.

## Exploitation

**Endpoint:**

```http
POST /forgot-password?temp-forgot-password-token=
```

**Modified request parameters:**

```http
temp-forgot-password-token=
username=carlos
new-password=NewPassword123
```

The reset token was removed, and the username parameter was changed to the victim's username.

The application accepted the request without properly validating the reset token, allowing the password to be changed.

## Vulnerability

The password reset functionality failed to validate the password reset token before processing password changes.

Additionally, the application trusted a user-controlled username parameter, allowing an attacker to modify the password of another account.

## Impact

An attacker could exploit this weakness to:

* Reset passwords belonging to other users.
* Take over user accounts without access to their email.
* Bypass the intended password recovery process.
* Gain unauthorized access to sensitive account information.

## Remediation

* Validate password reset tokens on the server before processing password changes.
* Bind each reset token to a specific user account.
* Ensure tokens are random, unique, and time-limited.
* Invalidate tokens after successful use.
* Never trust user-controlled username parameters during password recovery.
* Require proper authorization for every password reset operation.

## Evidence

<img width="1582" height="740" alt="image" src="https://github.com/user-attachments/assets/bc129b80-1a16-4c08-93b3-5ece5afbb13d" />


## Result

Successfully exploited the password reset functionality by removing the reset token and modifying the username parameter. Changed Carlos's password and authenticated successfully to his account.
