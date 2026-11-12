# Lab: Reflected XSS with Some SVG Markup Allowed

## Lab Information

- **Platform:** PortSwigger Web Security Academy
- **Difficulty:** Practitioner
- **Category:** Cross-Site Scripting (XSS)
- **Status:** Solved
- **Vulnerability Type:** Reflected Cross-Site Scripting
- **Tools:** Firefox, Burp Suite

## Objective

Exploit a reflected XSS vulnerability by identifying which SVG tags and event-handler attributes are permitted by the application's input filtering.

The objective is to bypass the filtering mechanism and execute JavaScript using an allowed SVG element and event handler.

## Methodology

### 1. Initial Payload Testing

Started by testing a standard XSS payload:

```html
<img src=1 onerror=alert(1)>
```

The application blocked the payload, indicating that input filtering was in place.

### 2. Identifying Allowed HTML Tags

1. Opened the lab using Burp Suite's browser.
2. Submitted a search request and sent it to Burp Intruder.
3. Replaced the search parameter value with `<>`.
4. Marked the position between the angle brackets as the payload position.
5. Used the PortSwigger XSS Cheat Sheet to obtain a list of HTML tags.
6. Loaded the tag list into Burp Intruder and started the attack.

**Results:**

Most tested tags returned HTTP `400`, while the following tags returned HTTP `200`:

- `<svg>`
- `<animatetransform>`
- `<title>`
- `<image>`

This indicated that these tags were accepted by the application's filtering mechanism.

### 3. Identifying Allowed Event Handlers

Next, tested which event-handler attributes were accepted.

Used the following request template:

```html
<svg><animatetransform%20§§=1>
```

1. Marked the attribute position as the Intruder payload position.
2. Copied the event-handler list from the PortSwigger XSS Cheat Sheet.
3. Loaded the list into Burp Intruder.
4. Started the attack and reviewed the HTTP responses.

**Results:**

Most tested event handlers returned HTTP `400`, except for `onbegin`, which returned HTTP `200`.

This identified an allowed event handler that could be used to execute JavaScript.

### 4. Exploitation

Combined the allowed SVG markup with the accepted event handler.

**Final payload:**

```html
"><svg><animatetransform onbegin=alert(1)>
```

**URL-encoded search parameter:**

```text
%22%3E%3Csvg%3E%3Canimatetransform%20onbegin=alert(1)%3E
```

Visited the following URL:

```text
[https://YOUR-LAB-ID.web-security-academy.net/?search=%22%3E%3Csvg%3E%3Canimatetransform%20onbegin=alert(1)%3E](https://0a10007b033829df80d417af00420019.h1-web-security-academy.net/?search=%22%3E%3Csvg%3E%3Canimatetransform%20onbegin=alert(1)%3E)
```

The browser executed the JavaScript payload, triggering the alert and solving the lab.

## Vulnerability Explanation

The application attempted to prevent XSS by filtering HTML tags and event-handler attributes.

However, the filtering mechanism used an incomplete allowlist/blocklist approach. Certain SVG elements and event handlers remained permitted.

By combining the allowed `<svg>` and `<animatetransform>` elements with the `onbegin` event handler, it was possible to execute JavaScript despite the filtering restrictions.

## Impact

An attacker could potentially exploit this vulnerability to execute arbitrary JavaScript in a victim's browser.

Potential consequences include:

- Unauthorized actions within the victim's authenticated session.
- Access to sensitive information exposed to client-side JavaScript.
- Manipulation of application content.
- Phishing attacks through injected content.

## Remediation

- Use a well-maintained HTML sanitization library.
- Apply strict allowlists for permitted HTML and SVG elements.
- Remove dangerous event-handler attributes such as `onbegin`.
- Apply context-aware output encoding to untrusted input.
- Implement a restrictive Content Security Policy (CSP) as an additional security layer.
- Avoid relying exclusively on blacklist-based filtering.

## Evidence

<img width="1297" height="850" alt="Screenshot 2026-11-12 085427" src="https://github.com/user-attachments/assets/902cfb91-5411-4cb4-a048-79e7ca2f0a1d" />
<img width="1297" height="850" alt="Screenshot 2026-11-12 085427-00" src="https://github.com/user-attachments/assets/d63eb796-293b-48a4-8b1a-ef0cd1c9f77c" />
<img width="1221" height="642" alt="image" src="https://github.com/user-attachments/assets/cfcb2ac9-f86e-4f42-9d72-e45d68c5a96e" />
<img width="1512" height="280" alt="image" src="https://github.com/user-attachments/assets/4c84321a-462f-4f54-9a53-b052b772ea59" />



## Key Learning

Effective XSS testing requires more than testing standard payloads. Burp Intruder can help systematically identify accepted HTML elements and event-handler attributes. SVG markup provides alternative execution contexts that may bypass incomplete filtering mechanisms.

An HTTP `200` response alone does not prove XSS. The final payload must be tested to confirm actual JavaScript execution.

## Lab Reference

https://portswigger.net/web-security/cross-site-scripting/contexts/lab-some-svg-markup-allowed
