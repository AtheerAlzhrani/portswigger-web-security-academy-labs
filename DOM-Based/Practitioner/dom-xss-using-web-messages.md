# Lab: DOM XSS Using Web Messages

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Practitioner  
**Category:** DOM-Based Vulnerabilities  
**Status:** Solved  
**Tool:** Firefox, PortSwigger Exploit Server

## Objective

Exploit a DOM-based Cross-Site Scripting (DOM XSS) vulnerability by sending a malicious web message to the target application, causing the browser to execute the JavaScript `print()` function.

## Vulnerability Overview

The application contains a JavaScript event listener that listens for incoming messages using the `window.addEventListener('message', ...)` event.

The application processes the received message and inserts its content into the page using a DOM manipulation method.

Because the application does not properly validate or sanitize incoming messages, an attacker can send malicious HTML through the `postMessage()` API.

This allows injected HTML and JavaScript event handlers to execute in the context of the target application.

## Methodology

1. Inspected the application's homepage using browser DevTools.
2. Identified a JavaScript `addEventListener()` function listening for incoming web messages.
3. Observed that the application processes incoming messages and inserts their content into the `ads` HTML element.
4. Opened the PortSwigger Exploit Server.
5. Created an iframe pointing to the vulnerable target application.
6. Used the iframe's `onload` event to send a malicious message using `postMessage()`.
7. Stored the exploit and delivered it to the victim.
8. Successfully triggered the `print()` function.

## Payload

```html
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/"
onload="this.contentWindow.postMessage('<img src=1 onerror=print()>','*')">
</iframe>
```

## Technical Explanation

The exploit works through the following sequence:

1. The iframe loads the vulnerable application.
2. The `onload` event executes after the iframe finishes loading.
3. `postMessage()` sends the malicious HTML to the target window.
4. The target application's message event listener receives the message.
5. The application inserts the message content into the `ads` element.
6. The browser attempts to load the image from the invalid source `src=1`.
7. The image loading fails, triggering the `onerror` event.
8. The JavaScript `print()` function executes.

### Key JavaScript Concepts

- `window.addEventListener('message', ...)`: Receives messages from other windows.
- `postMessage()`: Allows communication between different browsing contexts.
- `event.data`: Contains the received message data.
- `innerHTML`: Parses and inserts supplied content as HTML.
- `onerror`: Executes JavaScript when a resource fails to load.

## Security Impact

An attacker may exploit insecure web message handling to inject malicious HTML and execute JavaScript in the context of the vulnerable application.

Depending on the application's functionality and the victim's privileges, this could potentially lead to:

- Unauthorized actions performed through the victim's authenticated session.
- Manipulation of page content.
- Phishing attacks through injected content.
- Access to sensitive information exposed to JavaScript.

## Remediation

1. Validate incoming messages before processing them.
2. Verify the sender's origin using `event.origin`.
3. Avoid inserting untrusted message data using `innerHTML`.
4. Use `textContent` when inserting plain text.
5. Sanitize HTML when HTML rendering is necessary.
6. Implement a strict Content Security Policy (CSP) as an additional security layer.

## Evidence
<img width="1637" height="322" alt="image" src="https://github.com/user-attachments/assets/7c029e39-0af1-4ac4-bd6e-8e6655004fe1" />


## Reference

 [PortSwigger: DOM XSS Using Web Messages](https://portswigger.net/web-security/dom-based/controlling-the-web-message-source/lab-dom-xss-using-web-messages)

