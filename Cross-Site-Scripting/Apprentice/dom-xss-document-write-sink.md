# DOM XSS in `document.write` Sink

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Apprentice  
**Category:** Cross-Site Scripting (DOM-based XSS)  
**Status:** Solved  
**Tool(s):** Burp Suite, Browser Developer Tools

## Objective

Exploit a DOM-based cross-site scripting vulnerability in the search query tracking functionality by injecting JavaScript that executes the `alert()` function.

## Methodology

1. Opened the lab and entered a random alphanumeric string into the search box.
2. Inspected the page using Browser Developer Tools to identify where the input was inserted into the HTML.
3. Observed that the search input was placed inside an `img` element's `src` attribute.
4. Identified the vulnerable JavaScript flow:
   - **Source:** `location.search`
   - **Sink:** `document.write()`
5. Crafted a payload to break out of the existing HTML attribute and inject an SVG element with an event handler.
6. Submitted the payload through the search functionality.
7. Successfully triggered the JavaScript `alert()` function, solving the lab.

## Payload

```html
"><svg onload=alert(1)>
```

## Vulnerability

The application uses `document.write()` to insert data from `location.search` into the page.

Because the input is not safely handled before being written into the HTML document, an attacker can escape the existing `img src` attribute and inject additional HTML elements containing executable JavaScript.

This creates a DOM-based XSS vulnerability.

## Impact

Successful exploitation could allow an attacker to execute JavaScript in a victim's browser within the vulnerable website's origin.

Depending on the application, this could lead to:
- Unauthorized actions performed in the victim's session.
- Access to sensitive information available to client-side JavaScript.
- Manipulation of page content.
- Phishing attacks through injected content.

## Remediation

- Avoid using `document.write()` with untrusted input.
- Use safer DOM APIs such as `textContent` when displaying user-controlled data.
- Apply context-appropriate output encoding.
- Validate and sanitize input where appropriate.
- Implement a restrictive Content Security Policy (CSP) as an additional security layer.

## Evidence

**Screenshot 1 – Search input:**  
`[Insert screenshot showing the initial search input]`

**Screenshot 2 – HTML inspection:**  
`[Insert screenshot showing the input inside the img src attribute]`

**Screenshot 3 – Successful XSS execution:**  
`[Insert screenshot showing the alert(1) popup]`

