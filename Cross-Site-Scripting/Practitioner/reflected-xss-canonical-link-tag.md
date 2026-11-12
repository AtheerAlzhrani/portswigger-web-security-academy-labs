# Lab: Reflected XSS in Canonical Link Tag

## Lab Information

- **Platform:** PortSwigger Web Security Academy
- **Difficulty:** Practitioner
- **Category:** Cross-Site Scripting (XSS)
- **Status:** Solved
- **Vulnerability Type:** Reflected Cross-Site Scripting
- **Tools:** Browser (Google Chrome), Burp Suite

## Objective

Exploit a reflected XSS vulnerability in the canonical link tag by injecting an additional HTML attribute that executes JavaScript.

The application escapes angle brackets, preventing the injection of new HTML elements. The objective is to bypass this restriction by injecting an event-handler attribute and triggering the `alert()` function using a keyboard shortcut.

## Methodology

1. Opened the lab home page and examined how user-controlled input was reflected in the canonical link tag.
2. Identified that angle brackets were escaped, preventing standard HTML tag injection.
3. Tested whether the reflected input could break out of the existing HTML attribute context.
4. Injected additional attributes through the URL query string.
5. Used the `accesskey` attribute to assign the `X` key as a keyboard shortcut.
6. Added an `onclick` event handler to execute JavaScript when the access key is activated.
7. Opened the crafted URL in Google Chrome.
8. Pressed the appropriate keyboard combination to trigger the JavaScript alert.

## Payload

**Exploit URL:**

```text
https://YOUR-LAB-ID.web-security-academy.net/?%27accesskey=%27x%27onclick=%27alert(1)
```

Replace `YOUR-LAB-ID` with the actual lab identifier.

**Decoded injected input:**

```text
'accesskey='x'onclick='alert(1)
```

The injected attributes effectively modify the canonical link element to include:

```html
accesskey='x' onclick='alert(1)'
```

## Vulnerability Explanation

The application reflects user-controlled input inside the `href` attribute of a canonical link element.

Although angle brackets are escaped, the application does not properly prevent attribute injection.

By injecting a single quote (`'`), it is possible to break out of the existing attribute value and introduce additional attributes.

The `accesskey` attribute assigns a keyboard shortcut to the element, while the `onclick` event handler executes JavaScript when the shortcut is activated.

## Exploitation

After visiting the crafted URL, trigger the injected event handler using the following keyboard combinations:

| Operating System | Keyboard Shortcut |
|---|---|
| Windows | `ALT + SHIFT + X` |
| macOS | `CTRL + ALT + X` |
| Linux | `ALT + X` |

The browser triggers the `onclick` event handler, executing:

```javascript
alert(1)
```

The lab is designed to work in Google Chrome.

## Impact

An attacker could exploit this vulnerability to execute arbitrary JavaScript in a victim's browser.

Potential consequences include:

- Unauthorized actions within the victim's authenticated session.
- Access to sensitive information exposed to client-side JavaScript.
- Manipulation of application content.
- Phishing attacks through injected content.

## Remediation

- Apply context-aware output encoding for HTML attributes.
- Properly encode special characters, particularly quotes, before inserting user input into attribute values.
- Use safe templating mechanisms that automatically encode untrusted input.
- Avoid inserting untrusted input directly into HTML attributes.
- Implement a restrictive Content Security Policy (CSP) as an additional security layer.

## Evidence

- [ ] Screenshot showing the crafted URL.
- [ ] Screenshot showing the injected canonical link attributes.
- [ ] Screenshot of the JavaScript alert.
- [ ] Screenshot of the solved lab confirmation.

## Key Learning

Escaping angle brackets alone is not sufficient to prevent XSS.

When user input is reflected inside an HTML attribute, an attacker may be able to break out of the existing attribute value using quote characters and inject additional attributes.

This lab demonstrates the importance of understanding HTML contexts, attribute injection, event handlers, and browser-specific keyboard access keys.

## Lab Reference

[PortSwigger – Reflected XSS in Canonical Link Tag](https://portswigger.net/web-security/cross-site-scripting/contexts/lab-canonical-link-tag)
