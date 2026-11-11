# Reflected DOM XSS

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Practitioner  
**Category:** Reflected DOM-Based Cross-Site Scripting (XSS)  
**Status:** Solved  
**Tools:** Burp Suite, Firefox

## Objective

Exploit a reflected DOM-based XSS vulnerability caused by unsafe handling of a JSON response that is processed using JavaScript's `eval()` function.

## Methodology

1. Enabled Intercept in Burp Suite Proxy.
2. Submitted a random test string (`XSS`) through the website's search functionality.
3. Intercepted the search request and forwarded it to the server.
4. Examined the intercepted response and identified that the search term was reflected inside a JSON response named `search-results`.
5. Inspected `searchResults.js` through the Burp Suite Site Map.
6. Identified that the JSON response was processed using an `eval()` function.
7. Experimented with different search inputs and observed that quotation marks were escaped, but backslashes were not properly escaped.
8. Crafted a payload to escape the JavaScript string context and inject an executable expression.
9. Submitted the following payload through the search functionality.
10. Successfully triggered a JavaScript alert, confirming the vulnerability.

## Payload

```javascript
\"-alert(1)}//
```

## Vulnerability

The application reflected user-controlled search input inside a JSON response.

The JavaScript file `searchResults.js` subsequently processed this response using the `eval()` function.

Although quotation marks were escaped, backslashes were not properly handled.

This allowed an attacker to manipulate the JavaScript parsing context and inject an executable expression.

The vulnerability occurred because untrusted data was processed using an unsafe JavaScript evaluation mechanism.

## Impact

An attacker could potentially:

- Execute arbitrary JavaScript in a victim's browser.
- Manipulate the application's DOM and page content.
- Perform unauthorized actions within the victim's session.
- Access sensitive information exposed to client-side JavaScript.
- Conduct phishing attacks.

## Remediation

- Avoid using `eval()` to process JSON responses.
- Use `JSON.parse()` instead of dynamically evaluating JSON data.
- Apply proper context-aware encoding.
- Ensure backslashes and other special characters are handled correctly.
- Treat all user-controlled input as untrusted.
- Implement a restrictive Content Security Policy (CSP) as an additional security layer.

## Evidence


<img width="1341" height="732" alt="image" src="https://github.com/user-attachments/assets/ca70c6ff-0406-4272-bdaa-4795b19f5594" />
<img width="507" height="210" alt="image" src="https://github.com/user-attachments/assets/c79ca8dc-336e-46e8-b6bd-ea62b9e9a34e" />

<img width="1116" height="245" alt="image" src="https://github.com/user-attachments/assets/d6773677-51f8-4c6d-b7ed-b47c7307147c" />



## Lab Reference

[PortSwigger Web Security Academy – Reflected DOM XSS.](https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-dom-xss-reflected)

