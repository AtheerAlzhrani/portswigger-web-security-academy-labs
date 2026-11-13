# Lab: Reflected XSS into a JavaScript String with Single Quote and Backslash Escaped

## Lab Information

- **Platform:** PortSwigger Web Security Academy
- **Difficulty:** Practitioner
- **Category:** Cross-Site Scripting (XSS)
- **Status:** Solved
- **Vulnerability Type:** Reflected Cross-Site Scripting
- **Tools:** Burp Suite, Burp Repeater, Browser

## Objective

Exploit a reflected XSS vulnerability in the search query tracking functionality.

The application reflects user-controlled input inside a JavaScript string and escapes single quotes and backslashes.

The objective is to bypass this protection, break out of the JavaScript context, and execute JavaScript using the `alert()` function.

## Methodology

### 1. Identify the Reflection

1. Submitted a random alphanumeric string through the search functionality.
2. Intercepted the request using Burp Suite.
3. Sent the request to Burp Repeater.
4. Examined the response and identified that the search term was reflected inside a JavaScript string.

### 2. Test Input Escaping

Submitted the following test input:

```text
test'payload
```

Observed that the single quote was escaped with a backslash in the JavaScript context.

This prevented the payload from directly breaking out of the existing string.

### 3. Bypass the JavaScript Context

Instead of attempting to escape the JavaScript string using a single quote, used the HTML closing script tag to terminate the surrounding script block.

**Payload:**

```html
</script><script>alert(1)</script>
```

This approach escapes the HTML script element context rather than relying on breaking the JavaScript string.

### 4. Verify the Exploit

1. Submitted the payload through Burp Repeater.
2. Copied the resulting URL.
3. Opened the URL directly in the browser.
4. Confirmed that the JavaScript alert was executed.
5. Verified that the lab was solved.

## Payload

**Final payload:**

```html
</script><script>alert(1)</script>
```

## Vulnerability Explanation

The application reflected user-controlled input inside a JavaScript string within an HTML `<script>` element.

Although single quotes and backslashes were escaped, the application did not safely handle the HTML script-element context.

The HTML parser recognizes `</script>` as the closing tag even when it appears inside a JavaScript string.

This allows an attacker to terminate the existing script block and inject a new script element containing malicious JavaScript.

## Impact

Successful exploitation could allow an attacker to execute arbitrary JavaScript in a victim's browser.

Potential consequences include:

- Unauthorized actions within the victim's authenticated session.
- Access to sensitive information exposed to client-side JavaScript.
- Manipulation of application content.
- Phishing attacks through injected content.

## Remediation

- Apply context-aware output encoding based on where untrusted data is inserted.
- Avoid directly embedding user-controlled input inside JavaScript code.
- Use safe serialization techniques when transferring data from server-side code to JavaScript.
- Prefer external JavaScript files and DOM APIs that treat input as text.
- Implement a restrictive Content Security Policy (CSP) as an additional security layer.

## Evidence

- [ ] Screenshot showing the reflected search term inside the JavaScript string.
- [ ] Screenshot showing the escaped single quote.
- [ ] Screenshot of the final payload.
- [ ] Screenshot of successful JavaScript execution.
- [ ] Screenshot of the solved lab confirmation.

## Key Learning

Escaping JavaScript string characters alone does not guarantee protection against XSS.

When user input is embedded inside an HTML `<script>` element, the HTML parsing context must also be considered. A closing `</script>` sequence can terminate the script element before the JavaScript parser evaluates the remaining content.

## Lab Reference

[PortSwigger – Reflected XSS into a JavaScript string with single quote and backslash escaped](https://portswigger.net/web-security/cross-site-scripting/contexts/lab-javascript-string-single-quote-backslash-escaped)
