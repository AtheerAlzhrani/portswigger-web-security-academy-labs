# Reflected XSS into HTML Attribute with Angle Brackets HTML-Encoded

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Apprentice  
**Category:** Reflected Cross-Site Scripting (XSS)  
**Status:** Solved  
**Tools:** Burp Suite, Burp Repeater, Web Browser

## Objective

Exploit a reflected XSS vulnerability by injecting JavaScript into an HTML attribute, escaping the existing quoted attribute and triggering an alert through an event handler.

## Methodology

1. Submitted a random alphanumeric string into the website's search functionality.
2. Intercepted the search request using Burp Suite.
3. Sent the intercepted request to Burp Repeater for further testing.
4. Examined the server response and identified that the input was reflected inside a quoted HTML attribute.
5. Replaced the original input with a payload designed to escape the existing attribute and inject an event handler.
6. Copied the generated URL and opened it in the browser.
7. Moved the mouse pointer over the injected element to trigger the JavaScript alert.

## Payload

```html
"onmouseover="alert(1)
```

## Vulnerability

The application reflected user-controlled input inside a quoted HTML attribute without properly encoding it for the HTML attribute context.

By injecting a double quote, it was possible to escape the original attribute and introduce a new event handler (`onmouseover`).

When the user hovered over the affected element, the browser executed the injected JavaScript.

## Impact

An attacker could potentially exploit this vulnerability to execute arbitrary JavaScript in the victim's browser within the security context of the vulnerable website.

Depending on the application, this could lead to:

- Unauthorized actions performed on behalf of the victim.
- Manipulation of page content.
- Phishing attacks through injected content.
- Access to sensitive information exposed to JavaScript.

## Remediation

- Apply context-aware output encoding before reflecting user-controlled input.
- Properly encode special characters in HTML attribute contexts.
- Avoid inserting untrusted input directly into HTML attributes.
- Implement a restrictive Content Security Policy (CSP) as an additional security layer.
- Use secure templating mechanisms that automatically encode output.

## Evidence

- [ ] Screenshot of the reflected input inside the HTML attribute.
- [ ] Screenshot of the injected payload in Burp Repeater.
- [ ] Screenshot of the JavaScript alert successfully triggered.

## Lab Reference

PortSwigger Web Security Academy – Reflected XSS into HTML attribute with angle brackets HTML-encoded.
