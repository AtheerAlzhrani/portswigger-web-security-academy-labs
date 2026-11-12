# Stored DOM XSS

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Practitioner  
**Category:** Stored DOM-Based Cross-Site Scripting (XSS)  
**Status:** Solved  
**Tools:** Web Browser, Browser Developer Tools

## Objective

Exploit a stored DOM-based XSS vulnerability in the blog comment functionality by bypassing an ineffective HTML encoding mechanism and executing JavaScript through an injected HTML element.

## Methodology

1. Navigated to the blog post and opened the comment functionality.
2. Examined how the application processed and displayed submitted comments.
3. Identified that the application used JavaScript's `replace()` function to encode angle brackets.
4. Analyzed the filtering behavior and discovered that only the first occurrence of the specified string was replaced.
5. Crafted a payload containing an additional pair of angle brackets at the beginning.
6. Submitted the payload as a blog comment.
7. The first pair of angle brackets was encoded, while the subsequent HTML injection remained effective.
8. Successfully triggered a JavaScript alert when the stored comment was rendered.

## Payload

```html
<><img src=1 onerror=alert(1)>
```

## Vulnerability

The application contained an ineffective client-side filtering mechanism that used JavaScript's `replace()` function to encode angle brackets.

When the first argument to `replace()` is a string rather than a global regular expression, only the first matching occurrence is replaced.

By placing an additional pair of angle brackets at the beginning of the comment, the first pair was encoded while the subsequent HTML tags remained unaffected.

The injected image element used an invalid source, triggering the `onerror` event handler and executing JavaScript.

## Impact

An attacker could potentially:

- Execute arbitrary JavaScript in visitors' browsers.
- Perform unauthorized actions within victims' sessions.
- Manipulate blog content and page behavior.
- Access sensitive information exposed to client-side JavaScript.
- Deliver malicious content to multiple visitors viewing the affected comment.

## Remediation

- Avoid relying on client-side string replacement for HTML sanitization.
- Use robust HTML sanitization libraries.
- Apply context-aware output encoding.
- Use safe DOM APIs such as `textContent` when rendering untrusted text.
- Avoid inserting untrusted content using unsafe HTML sinks.
- Implement a restrictive Content Security Policy (CSP) as an additional security layer.

## Evidence

- [ ] Screenshot of the submitted comment containing the payload.
- [ ] Screenshot showing the injected HTML element in the DOM.
- [ ] Screenshot of the successful JavaScript alert.

## Lab Reference

PortSwigger Web Security Academy – Stored DOM XSS.
