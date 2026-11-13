# Lab: Reflected XSS into a JavaScript String with Angle Brackets and Double Quotes HTML-Encoded and Single Quotes Escaped

## Lab Information

- **Platform:** PortSwigger Web Security Academy
- **Difficulty:** Practitioner
- **Category:** Cross-Site Scripting (XSS)
- **Status:** Solved
- **Vulnerability Type:** Reflected Cross-Site Scripting
- **Tools:** Burp Suite, Firefox

## Objective

Exploit a reflected XSS vulnerability in the search query tracking functionality.

The application HTML-encodes angle brackets and double quotes and escapes single quotes.

The objective is to bypass these protections, break out of the JavaScript string, and execute JavaScript using the `alert()` function.

## Methodology

### 1. Identify the Reflection

1. Submitted a random alphanumeric string through the search functionality.
2. Intercepted the search request using Burp Suite.
3. Sent the request to Burp Repeater.
4. Examined the response and confirmed that the search term was reflected inside a JavaScript string.

### 2. Test Input Escaping

Tested the following payload:

```text
test'payload
```

Observed that the single quote was escaped with a backslash, preventing a direct breakout from the JavaScript string.

Next, tested:

```text
test\payload
```

Observed that the backslash was not escaped by the application.

This revealed an inconsistency in the application's input handling.

### 3. Bypass the Protection

Used the unescaped backslash to interfere with the application's single-quote escaping mechanism.

**Final payload:**

```javascript
\'-alert(1)//
```

The application escapes the single quote by adding a backslash. However, because the original backslash is not escaped, the resulting JavaScript contains two consecutive backslashes before the quote.

The JavaScript parser interprets the two backslashes as an escaped backslash, allowing the single quote to terminate the original string.

The `-alert(1)` expression then executes JavaScript, while `//` comments out the remaining characters on the line.

### 4. Verify the Exploit

1. Submitted the payload through Burp Repeater.
2. Copied the resulting URL.
3. Opened the URL directly in the browser.
4. Confirmed that the JavaScript alert was executed.
5. Verified that the lab was solved.

## Payload

**Final payload:**

```javascript
\'-alert(1)//
```

## Vulnerability Explanation

The application reflects user-controlled input inside a JavaScript string.

Although it HTML-encodes angle brackets and double quotes and escapes single quotes, it does not properly escape backslashes.

This creates an escaping inconsistency.

By supplying a backslash before a single quote, the attacker can cause the application's escaping operation to produce two consecutive backslashes. The JavaScript parser interprets these as an escaped backslash, allowing the following single quote to terminate the original string.

This enables JavaScript execution within the reflected script context.

## Impact

Successful exploitation could allow an attacker to execute arbitrary JavaScript in a victim's browser.

Potential consequences include:

- Unauthorized actions within the victim's authenticated session.
- Access to sensitive information exposed to client-side JavaScript.
- Manipulation of application content.
- Phishing attacks through injected content.

## Remediation

- Apply context-aware output encoding when embedding untrusted input in JavaScript.
- Properly escape backslashes before quotes to prevent escaping inconsistencies.
- Avoid directly concatenating user-controlled input into JavaScript strings.
- Use safe JSON serialization when transferring data into JavaScript.
- Prefer DOM APIs that treat untrusted input as text.
- Implement a restrictive Content Security Policy (CSP) as an additional security layer.

## Evidence

<img width="1292" height="932" alt="image" src="https://github.com/user-attachments/assets/b2defd43-5093-4720-9eae-757be48c510d" />
<img width="1632" height="267" alt="image" src="https://github.com/user-attachments/assets/80d54aaa-3af1-4fac-816a-322ec071a3f9" />



## Key Learning

Escaping quotes alone is not sufficient to prevent JavaScript-based XSS.

Backslashes must also be handled correctly. An attacker can exploit inconsistent escaping to neutralize quote protection and break out of a JavaScript string.

This lab demonstrates the importance of testing how different special characters interact with one another rather than testing each character independently.

## Lab Reference

[PortSwigger – Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped](https://portswigger.net/web-security/cross-site-scripting/contexts/lab-javascript-string-angle-brackets-double-quotes-encoded-single-quotes-escaped)

