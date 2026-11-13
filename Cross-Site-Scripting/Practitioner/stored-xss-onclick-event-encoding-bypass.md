# Lab: Stored XSS into Onclick Event with Angle Brackets and Double Quotes HTML-Encoded and Single Quotes and Backslash Escaped

## Lab Information

- **Platform:** PortSwigger Web Security Academy
- **Difficulty:** Practitioner
- **Category:** Cross-Site Scripting (XSS)
- **Status:** Solved
- **Vulnerability Type:** Stored Cross-Site Scripting
- **Tools:** Burp Suite, Firefox

## Objective

Exploit a stored XSS vulnerability in the comment functionality.

The application reflects the submitted website value inside an `onclick` event-handler attribute. Although angle brackets and double quotes are HTML-encoded and single quotes and backslashes are escaped, the objective is to bypass these protections and execute JavaScript when a visitor clicks the comment author's name.

## Methodology

### 1. Identify the Input Reflection

1. Opened the lab and navigated to a blog post with a comment form.
2. Entered a random alphanumeric string in the Website field.
3. Submitted the comment.
4. Intercepted the submission request using Burp Suite and sent it to Burp Repeater.
5. Made a second request to view the blog post containing the submitted comment.
6. Sent the viewing request to another Burp Repeater tab.
7. Examined the response and identified that the Website value was reflected inside an `onclick` event-handler attribute.

### 2. Analyze the Filtering

The application applied several encoding and escaping protections:

- Angle brackets were HTML-encoded.
- Double quotes were HTML-encoded.
- Single quotes were escaped.
- Backslashes were escaped.

However, the application did not correctly handle HTML character references in the JavaScript event-handler context.

### 3. Bypass the Protection

Modified the Website field to use the following payload:

```text
http://foo?&apos;-alert(1)-&apos;
```

The `&apos;` character reference is decoded by the HTML parser into a single quote.

This allows the injected JavaScript expression to break out of the existing string context and execute `alert(1)`.

### 4. Verify the Exploit

1. Submitted the comment containing the payload.
2. Opened the blog post containing the stored comment.
3. Right-clicked the page and selected Copy URL.
4. Opened the copied URL in the browser.
5. Clicked the comment author's name.
6. Confirmed that the JavaScript alert was triggered and the lab was solved.

## Payload

**Website field:**

```text
http://foo?&apos;-alert(1)-&apos;
```

**JavaScript expression introduced through the HTML entity:**

```javascript
'-alert(1)-'
```

## Vulnerability Explanation

The application stores user-controlled comment data and later renders it inside an HTML `onclick` event-handler attribute.

Although the application attempts to escape special characters, it does not safely handle HTML character references in this context.

The browser decodes `&apos;` into a single quote during HTML parsing. This allows the injected quote characters to affect the JavaScript code inside the event handler.

As a result, clicking the comment author's name executes the injected JavaScript.

Because the malicious input is stored in the comment, the vulnerability can affect other visitors who view and interact with the comment.

## Impact

Successful exploitation could allow an attacker to execute arbitrary JavaScript in visitors' browsers.

Potential consequences include:

- Unauthorized actions within visitors' authenticated sessions.
- Access to sensitive information exposed to client-side JavaScript.
- Manipulation of application content.
- Phishing attacks through injected content.
- Attacks against multiple visitors who interact with the stored comment.

## Remediation

- Apply context-aware output encoding for HTML attributes and JavaScript event handlers.
- Avoid inserting untrusted user input directly into event-handler attributes.
- Use safe DOM APIs and event listeners instead of inline JavaScript.
- Apply strict input validation to website URL fields.
- Use a well-maintained HTML sanitization library where HTML input is required.
- Implement a restrictive Content Security Policy (CSP) as an additional security layer.

## Evidence
<img width="1280" height="882" alt="image" src="https://github.com/user-attachments/assets/07e5bb2f-99ac-4e96-ad1a-db9b69631321" />

<img width="1577" height="267" alt="Screenshot 2026-11-13 122045" src="https://github.com/user-attachments/assets/a922e37a-d6d7-42db-881d-6cece1e956ea" />


## Key Learning

Stored XSS can occur inside JavaScript event-handler attributes, even when an application encodes HTML tags and escapes common special characters.

HTML character references can create unexpected parsing behavior when HTML and JavaScript contexts are mixed.

This lab demonstrates the importance of understanding browser parsing order and applying context-specific output encoding.

## Lab Reference

https://portswigger.net/web-security/cross-site-scripting/contexts/lab-onclick-event-angle-brackets-double-quotes-html-encoded-single-quotes-backslash-escaped

