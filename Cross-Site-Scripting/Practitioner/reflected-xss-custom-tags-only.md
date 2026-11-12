# Lab: Reflected XSS into HTML Context with All Tags Blocked Except Custom Ones

## Lab Information

- **Platform:** PortSwigger Web Security Academy
- **Difficulty:** Practitioner
- **Category:** Cross-Site Scripting (XSS)
- **Status:** Solved
- **Vulnerability Type:** Reflected Cross-Site Scripting
- **Tools:** Burp Suite, Browser, PortSwigger Exploit Server

## Objective

Exploit a reflected XSS vulnerability by injecting a custom HTML element because the application blocks all standard HTML tags.

The objective is to execute JavaScript automatically and trigger an alert displaying `document.cookie`.

## Methodology

1. Opened the lab and identified the search functionality.
2. Opened the Exploit Server to prepare the exploit.
3. Injected a custom HTML element into the search parameter.
4. Used an `onfocus` event handler to execute JavaScript when the element receives focus.
5. Used `tabindex=1` to make the custom element focusable.
6. Used the URL fragment `#x` to automatically focus the injected element when the page loads.
7. Stored the exploit and delivered it to the victim.
8. Confirmed successful JavaScript execution through the alert dialog.

## Payload

**Exploit Server HTML:**

```html
<script>
location = 'https://YOUR-LAB-ID.web-security-academy.net/?search=%3Cxss+id%3Dx+onfocus%3Dalert%28document.cookie%29%20tabindex=1%3E#x';
</script>
```

Replace `YOUR-LAB-ID` with the actual lab identifier.

**Decoded injection:**

```html
<xss id=x onfocus=alert(document.cookie) tabindex=1>
```

## Vulnerability Explanation

The application blocks standard HTML tags but allows custom HTML elements.

The injected `<xss>` element includes an `onfocus` event handler that executes JavaScript when the element receives focus.

The `tabindex=1` attribute makes the element focusable, while the `#x` URL fragment targets the element using its ID and triggers focus when the page loads.

This allows JavaScript execution without using standard HTML tags such as `<script>` or `<img>`.

## Impact

An attacker could exploit this vulnerability to execute arbitrary JavaScript in a victim's browser.

Depending on the application context, this could lead to:

- Unauthorized actions performed within the victim's session.
- Access to sensitive information available to client-side JavaScript.
- Manipulation of page content.
- Phishing attacks through injected content.

## Remediation

- Apply context-aware output encoding to all untrusted user input.
- Use a strict HTML sanitization library with an explicit allowlist of permitted elements and attributes.
- Remove dangerous event-handler attributes such as `onfocus`.
- Implement a restrictive Content Security Policy (CSP) as an additional security layer.
- Avoid relying only on blocking standard HTML tags to prevent XSS.

## Evidence

- [ ] Screenshot of the injected custom HTML element.
- [ ] Screenshot of the successful JavaScript alert.
- [ ] Screenshot of the solved lab confirmation.

## Key Learning

Blocking standard HTML tags is not sufficient to prevent XSS. Custom HTML elements can still contain executable event handlers. Testing should include alternative HTML elements and event-handler combinations when assessing HTML sanitization defenses.

## Lab Reference

[PortSwigger – Reflected XSS into HTML context with all tags blocked except custom ones](https://portswigger.net/web-security/cross-site-scripting/contexts/lab-html-context-with-all-standard-tags-blocked)
