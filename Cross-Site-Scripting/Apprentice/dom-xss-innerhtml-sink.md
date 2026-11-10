# DOM XSS in innerHTML Sink

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Apprentice  
**Category:** Cross-Site Scripting (DOM-based XSS)  
**Status:** Solved  
**Tool(s):** Firefox

## Objective

Exploit a DOM-based cross-site scripting vulnerability in the blog search functionality by injecting HTML that executes JavaScript and triggers the `alert()` function.

## Methodology

1. Opened the lab and located the blog search functionality.
2. Identified that the application uses the `innerHTML` property to modify a `div` element's HTML content.
3. Observed that the data comes from `location.search`, which is controlled through the URL query string.
4. Entered the following payload into the search box:

## Payload

```html
<img src=1 onerror=alert(1)>
```

5. Clicked the Search button.
6. The browser attempted to load the invalid image source, triggering the `onerror` event handler.
7. The `alert(1)` JavaScript executed successfully, solving the lab.

## Vulnerability

The application takes attacker-controlled input from `location.search` and inserts it into the page using the `innerHTML` property without appropriate sanitization.

Because `innerHTML` parses the supplied string as HTML, an attacker can inject HTML elements and event handlers that execute JavaScript.

**Source:** `location.search`  
**Sink:** `innerHTML`  
**Injection context:** HTML content  
**Event handler:** `onerror`

## Impact

Successful exploitation could allow an attacker to execute JavaScript in a victim's browser within the vulnerable website's origin.

Potential consequences include:
- Unauthorized actions performed through the victim's session.
- Access to sensitive information available to client-side JavaScript.
- Manipulation of page content.
- Phishing attacks through injected content.

## Remediation

- Avoid assigning untrusted input to `innerHTML`.
- Use `textContent` when displaying plain text.
- Apply appropriate HTML sanitization when HTML rendering is required.
- Validate and safely handle user-controlled input.
- Implement a restrictive Content Security Policy (CSP) as an additional security layer.

## Evidence

<img width="1180" height="437" alt="image" src="https://github.com/user-attachments/assets/3cee0ac1-2ee8-46a2-8064-0b6ba1f250e2" />

<img width="1267" height="857" alt="image" src="https://github.com/user-attachments/assets/e1308889-c07b-4445-9ca2-6256fece6c6d" />


