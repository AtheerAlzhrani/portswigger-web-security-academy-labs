# Reflected XSS into a JavaScript String

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Apprentice  
**Category:** Reflected Cross-Site Scripting (XSS)  
**Status:** Solved  
**Tools:** Burp Suite, Burp Repeater, Web Browser

## Objective

Exploit a reflected XSS vulnerability by breaking out of a JavaScript string and injecting JavaScript code to execute an alert.

## Methodology

1. Submitted a random alphanumeric string in the website's search functionality.
2. Intercepted the search request using Burp Suite.
3. Sent the request to Burp Repeater.
4. Examined the response and identified that the input was reflected inside a JavaScript string.
5. Replaced the original input with a payload designed to break out of the existing JavaScript string.
6. Copied the generated URL and opened it in the browser.
7. Successfully triggered a JavaScript alert when the page loaded.

## Payload

```javascript
'-alert(1)-'
```

## Vulnerability

The application reflected user-controlled input inside a JavaScript string without properly escaping or encoding it for the JavaScript context.

The injected single quote terminated the original JavaScript string, allowing the attacker to inject a JavaScript expression.

The `-alert(1)-` expression triggered the alert while maintaining valid JavaScript syntax.

## Impact

An attacker could potentially:

- Execute arbitrary JavaScript in a victim's browser.
- Manipulate page content and application behavior.
- Perform unauthorized actions using the victim's session.
- Access sensitive information exposed to client-side JavaScript.
- Conduct phishing attacks.

## Remediation

- Avoid inserting untrusted input directly into JavaScript code.
- Apply context-aware JavaScript output encoding.
- Use safe DOM APIs instead of dynamically constructing JavaScript.
- Validate and sanitize user-controlled input.
- Implement a restrictive Content Security Policy (CSP).

## Evidence

- [ ] Screenshot showing the reflected input inside the JavaScript string.
- [ ] Screenshot of the injected payload in Burp Repeater.
- [ ] Screenshot of the successful JavaScript alert.

## Lab Reference

PortSwigger Web Security Academy – Reflected XSS into a JavaScript string with single quote and backslash escaped.
