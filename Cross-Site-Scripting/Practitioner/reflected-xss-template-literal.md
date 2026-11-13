# Lab: Reflected XSS into a Template Literal

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Practitioner  
**Category:** Cross-Site Scripting (XSS)  
**Status:** Solved  
**Tool:** Firefox

## Objective

Exploit a reflected XSS vulnerability in the blog search functionality by injecting JavaScript into a template literal and triggering the `alert()` function.

## Methodology

1. Submitted a random alphanumeric string through the blog search functionality.
2. Intercepted the search request using Burp Suite and sent it to Repeater.
3. Examined the response and identified that the search input was reflected inside a JavaScript template literal.
4. Observed that angle brackets, single quotes, and double quotes were HTML-encoded, while backticks were escaped.
5. Tested a template literal expression to execute JavaScript without needing to break out of the string.
6. Used the following payload:

## Payload

```javascript
${alert(1)}
```

## Vulnerability

The application reflected user-controlled input inside a JavaScript template literal without safely neutralizing template literal expressions.

JavaScript template literals support expression interpolation using `${...}`. This means an attacker can inject an expression that JavaScript evaluates when the template literal is processed.

Although several characters were encoded or escaped, the application did not prevent the execution of an injected template expression.

## Impact

An attacker could potentially execute arbitrary JavaScript in a victim's browser under the vulnerable website's origin.

Depending on the application's functionality and security controls, this could lead to:
- Unauthorized actions performed in the victim's session.
- Access to sensitive information available to the page.
- Manipulation of page content.
- Further client-side attacks.

## Remediation

- Avoid inserting untrusted input directly into JavaScript template literals.
- Use context-aware output encoding.
- Avoid dynamically constructing JavaScript using user-controlled data.
- Prefer safe DOM APIs such as `textContent` instead of unsafe HTML or script insertion.
- Apply a restrictive Content Security Policy as an additional security layer.

## Evidence

<img width="1391" height="995" alt="image" src="https://github.com/user-attachments/assets/74821bc1-37d3-4d5f-b4bb-ac39b7eb2434" />
<img width="1595" height="250" alt="image" src="https://github.com/user-attachments/assets/532e6c00-cee9-4888-8bd8-fca461bd3353" />



## Lab Reference

https://portswigger.net/web-security/cross-site-scripting/contexts/lab-javascript-template-literal-angle-brackets-single-double-quotes-backslash-backticks-escaped
