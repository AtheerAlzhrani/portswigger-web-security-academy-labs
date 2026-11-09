# Reflected XSS into HTML Context with Nothing Encoded

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Apprentice  
**Category:** Reflected Cross-Site Scripting (XSS)  
**Status:** Solved  
**Tool:** Firefox, Burp Suite

## Objective

Exploit a reflected Cross-Site Scripting vulnerability in the search functionality by injecting JavaScript into the application's HTML response.

## Methodology

1. Opened the lab application.
2. Identified the search functionality as the vulnerable input point.
3. Entered a JavaScript payload into the search field.
4. Clicked the Search button.
5. Observed that the application reflected the submitted input into the HTML response without encoding.
6. Confirmed successful JavaScript execution through an alert dialog.

## Payload

```html
<script>alert(1)</script>
```

## Vulnerability

The application contains a reflected XSS vulnerability in its search functionality.

User-controlled input is reflected directly into the HTML response without proper output encoding. As a result, the browser interprets the injected script as executable JavaScript.

## Impact

An attacker could craft a malicious URL containing an XSS payload and trick a victim into opening it.

Successful exploitation could allow:
- JavaScript execution in the victim's browser.
- Modification of page content.
- Unauthorized actions within the victim's authenticated session, subject to application security controls.

## Remediation

- Implement context-aware output encoding.
- Avoid inserting untrusted input directly into HTML.
- Use safe templating mechanisms.
- Sanitize HTML when accepting user-supplied HTML is necessary.
- Implement Content Security Policy (CSP) as an additional security layer.

## Evidence

**Screenshot 1:** Search field containing the XSS payload.

`[Insert screenshot here]`

**Screenshot 2:** JavaScript alert successfully executed.

`[Insert screenshot here]`

**Screenshot 3:** HTTP response showing the reflected payload.

`[Insert screenshot here]`
