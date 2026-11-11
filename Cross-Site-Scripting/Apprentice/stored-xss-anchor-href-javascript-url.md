# Stored XSS into Anchor Href Attribute with Double Quotes HTML-Encoded

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Apprentice  
**Category:** Stored Cross-Site Scripting (XSS)  
**Status:** Solved  
**Tools:** Burp Suite, Burp Repeater, Web Browser

## Objective

Exploit a stored XSS vulnerability by injecting a JavaScript URL into the Website input field of a comment form.

## Methodology

1. Submitted a comment containing a random alphanumeric string in the Website field.
2. Intercepted the comment submission request using Burp Suite.
3. Sent the request to Burp Repeater.
4. Made another request in the browser to view the published comment.
5. Intercepted the second request and sent it to another Repeater tab.
6. Examined the response and identified that the Website input was stored and reflected inside an HTML anchor (`href`) attribute.
7. Submitted another comment using a JavaScript URL payload.
8. Opened the published comment and clicked the author's name.
9. Successfully triggered a JavaScript alert.

## Payload

```javascript
javascript:alert(1)
```

## Vulnerability

The application stored user-controlled input from the Website field and rendered it inside an HTML anchor's `href` attribute without properly validating the URL scheme.

By injecting a `javascript:` URL, an attacker could execute JavaScript when a visitor clicks the affected link.

Unlike reflected XSS, this vulnerability is persistent because the malicious input is stored by the application and served to subsequent visitors.

## Impact

An attacker could potentially:

- Execute JavaScript in other users' browsers.
- Manipulate website content.
- Perform unauthorized actions in the victim's session.
- Conduct phishing attacks.
- Access sensitive information available to client-side JavaScript.

## Remediation

- Validate URLs before storing or rendering them.
- Allow only trusted URL schemes such as `https:` and `http:`.
- Reject dangerous schemes such as `javascript:`.
- Apply context-aware output encoding.
- Sanitize user-controlled content before rendering it.
- Implement Content Security Policy (CSP) as an additional security layer.

## Evidence





- [ ] Screenshot of the Website input in the comment form.
- [ ] Screenshot showing the reflected URL inside the `href` attribute.
- [ ] Screenshot of the injected JavaScript URL.
- [ ] Screenshot of the successful JavaScript alert.

## Lab Reference

PortSwigger Web Security Academy – Stored XSS into anchor href attribute with double quotes HTML-encoded.https://portswigger.net/web-security/cross-site-scripting/contexts/lab-href-attribute-double-quotes-html-encodedhttps://portswigger.net/web-security/cross-site-scripting/contexts/lab-href-attribute-double-quotes-html-encoded
