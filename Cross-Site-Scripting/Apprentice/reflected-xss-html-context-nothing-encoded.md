# Reflected XSS into HTML Context with Nothing Encoded

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Apprentice  
**Category:** Reflected Cross-Site Scripting (XSS)  
**Status:** Solved  
**Tool:** Firefox

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


<img width="1522" height="612" alt="image" src="https://github.com/user-attachments/assets/e926c2aa-faff-45ad-a7ca-7398b7439012" />

<img width="1572" height="642" alt="image" src="https://github.com/user-attachments/assets/efc9ab22-3d57-4344-98e4-b31d03345cb0" />
<img width="1001" height="372" alt="image" src="https://github.com/user-attachments/assets/5a597e67-4884-4ee4-9e5c-c27d7478d45c" />
<img width="1691" height="682" alt="image" src="https://github.com/user-attachments/assets/69e1ed86-fba2-41a2-8e07-951d7b80f095" />

