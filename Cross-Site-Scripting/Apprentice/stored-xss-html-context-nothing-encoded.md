# Stored XSS into HTML Context with Nothing Encoded

**Platform:** PortSwigger Web Security Academy

**Difficulty:** Apprentice

**Category:** Stored Cross-Site Scripting (XSS)

**Status:** Solved

**Tools:** Firefox, Burp Suite

## Objective

Exploit a Stored Cross-Site Scripting vulnerability in the blog comment functionality by injecting JavaScript that executes when the blog post is viewed.

## Methodology

1. Opened the vulnerable blog post.
2. Identified the comment functionality as an input point.
3. Entered a JavaScript payload into the comment field.
4. Provided the required name, email, and website information.
5. Submitted the comment using the Post comment button.
6. Navigated back to the blog post.
7. Observed that the submitted comment was stored and rendered in the page.
8. Confirmed that the injected JavaScript executed when the blog post was viewed.

## Payload

```html
<script>alert(1)</script>
```

## Vulnerability

The application contains a Stored Cross-Site Scripting vulnerability in its comment functionality.

User-controlled input is stored by the application and subsequently rendered in the blog post without proper output encoding or sanitization.

As a result, the browser interprets the injected script as executable JavaScript whenever the affected blog post is viewed.

## Impact

Successful exploitation could allow an attacker to:

- Execute arbitrary JavaScript in visitors' browsers.
- Modify displayed page content.
- Perform unauthorized actions within authenticated sessions, subject to application security controls.
- Create persistent malicious content affecting multiple visitors.

## Remediation

- Implement context-aware output encoding when displaying user-generated content.
- Sanitize HTML if HTML input is intentionally supported.
- Validate user input on the server side.
- Avoid inserting untrusted content through unsafe DOM operations.
- Implement Content Security Policy (CSP) as an additional security layer.

## Evidence

<img width="1010" height="792" alt="image" src="https://github.com/user-attachments/assets/8d2e1609-d181-4709-bab5-1cc0a19ae689" />

<img width="1687" height="497" alt="image" src="https://github.com/user-attachments/assets/91117ae7-5abd-425a-877b-17f6cdbaa2cd" />

<img width="985" height="277" alt="image" src="https://github.com/user-attachments/assets/7f0a4e9f-0559-484e-8436-a158036c565b" />
<img width="757" height="311" alt="image" src="https://github.com/user-attachments/assets/7fbaad2a-4a07-4170-9c0f-01958974892e" />


<img width="1140" height="502" alt="image" src="https://github.com/user-attachments/assets/14358d7d-4d67-4e70-ab81-aff76211f351" />



