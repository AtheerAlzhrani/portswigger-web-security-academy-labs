# Lab: DOM-Based Cookie Manipulation

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Practitioner  
**Category:** DOM-Based Vulnerabilities  
**Status:** Solved  
**Tools:** Firefox, PortSwigger Exploit Server

## Objective

Exploit a DOM-based cookie manipulation vulnerability by injecting a malicious payload into the `lastViewedProduct` cookie, causing JavaScript execution when the victim visits the homepage and triggering the `print()` function.

## Vulnerability Overview

The application uses a client-side cookie named `lastViewedProduct` to store the URL of the last product page visited by the user.

The application retrieves this cookie and uses its value when processing page content.

Because the application does not properly validate or sanitize the cookie value, an attacker can manipulate the URL used to populate the cookie with malicious HTML and JavaScript.

When the victim subsequently visits the homepage, the application processes the manipulated cookie, resulting in DOM-based XSS.

## Methodology

1. Inspected the application's homepage and identified the `lastViewedProduct` cookie.
2. Observed that the cookie stores the URL of the last visited product page.
3. Identified that the application uses the cookie value when processing homepage content.
4. Opened the PortSwigger Exploit Server.
5. Created an iframe pointing to a product page with an injected JavaScript payload.
6. Used the iframe's `onload` event to navigate to the homepage after the initial request.
7. Stored the exploit and delivered it to the victim.
8. Successfully triggered the `print()` function through the manipulated cookie.

## Payload

```html
<iframe
src="https://YOUR-LAB-ID.web-security-academy.net/product?productId=1&'><script>print()</script>"
onload="if(!window.x)this.src='https://YOUR-LAB-ID.web-security-academy.net';window.x=1;">
</iframe>
```

## Technical Explanation

The exploit works through the following sequence:

1. The iframe initially loads the product page with the injected JavaScript payload.
2. The application stores the manipulated product URL in the `lastViewedProduct` cookie.
3. The iframe's `onload` event executes.
4. The `window.x` condition ensures that the iframe navigates to the homepage only once.
5. The iframe loads the homepage.
6. The homepage retrieves and processes the manipulated cookie value.
7. The injected HTML and JavaScript are interpreted by the browser.
8. The `print()` function executes, successfully solving the lab.

### Key JavaScript Concepts

- `document.cookie`: Allows JavaScript to access client-side cookies.
- `lastViewedProduct`: Stores the URL of the last visited product page.
- `iframe`: Loads another browsing context within the page.
- `onload`: Executes JavaScript when the iframe finishes loading.
- `window.x`: Prevents repeated navigation during the exploit.
- DOM manipulation: Allows JavaScript to dynamically modify page content.

## Security Impact

An attacker may exploit insecure client-side cookie handling to inject malicious HTML and execute JavaScript in the context of the vulnerable application.

Depending on the application's functionality and the victim's privileges, this could potentially lead to:

- DOM-based Cross-Site Scripting (XSS).
- Unauthorized actions through the victim's authenticated session.
- Manipulation of application content.
- Access to sensitive information exposed to JavaScript.

## Remediation

1. Validate cookie values before processing them.
2. Avoid inserting cookie values directly into HTML.
3. Use `textContent` instead of unsafe HTML manipulation methods.
4. Sanitize untrusted data whenever HTML rendering is necessary.
5. Implement appropriate cookie security attributes, including `Secure`, `HttpOnly`, and `SameSite` where applicable.
6. Apply a strict Content Security Policy (CSP) as an additional security layer.

## Evidence

<img width="1752" height="654" alt="image" src="https://github.com/user-attachments/assets/1bf4ff46-8f5d-440b-bbd9-b83dc7a3bd45" />


## Reference

[PortSwigger: DOM-Based Cookie Manipulation](https://portswigger.net/web-security/dom-based/cookie-manipulation/lab-dom-cookie-manipulation)
