# Lab: DOM XSS Using Web Messages and a JavaScript URL

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Practitioner  
**Category:** DOM-Based Vulnerabilities  
**Status:** Solved  
**Tools:** Firefox, PortSwigger Exploit Server

## Objective

Exploit a DOM-based vulnerability triggered through web messaging by bypassing weak URL validation and executing JavaScript through the `location.href` sink.

The objective was to construct an HTML page on the exploit server that sends a malicious web message to the vulnerable application and triggers the `print()` function.

## Vulnerability Overview

The application contains a JavaScript event listener that processes incoming messages using `window.addEventListener('message', ...)`.

The message handler performs an insecure validation check using `indexOf()` to determine whether the received message contains the strings `http:` or `https:`.

However, this validation only checks whether these strings appear anywhere in the message rather than verifying that the message contains a legitimate and safe URL.

The application subsequently assigns the received message to `location.href`, creating a dangerous DOM-based source-to-sink flow.

An attacker can exploit this behavior by combining a JavaScript URL with the expected validation string.

## Methodology

1. Inspected the application's homepage using browser DevTools.
2. Identified the `addEventListener('message', ...)` event listener.
3. Analyzed the message-handling logic.
4. Identified an insecure `indexOf()` validation check that searches for `http:` or `https:` within the received message.
5. Identified `location.href` as the dangerous DOM sink.
6. Opened the PortSwigger Exploit Server.
7. Created an iframe pointing to the vulnerable application.
8. Used the iframe's `onload` event to send a crafted message using `postMessage()`.
9. Included a JavaScript URL followed by the string `http:` to bypass the weak validation.
10. Stored the exploit and delivered it to the victim.
11. Successfully triggered the `print()` function.

## Exploit Payload

```html
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/"
onload="this.contentWindow.postMessage('javascript:print()//http:','*')">
</iframe>
```

## Technical Explanation

The exploit works through the following sequence:

1. The iframe loads the vulnerable application.
2. The `onload` event executes after the iframe finishes loading.
3. `postMessage()` sends the crafted JavaScript URL to the target window.
4. The application's message event listener receives the message.
5. The validation logic searches for `http:` using `indexOf()`.
6. Because the message contains `http:` after the JavaScript payload, the validation succeeds.
7. The application assigns the received message to `location.href`.
8. The browser interprets the URL as JavaScript.
9. The `//http:` portion is treated as a JavaScript line comment.
10. The `print()` function executes.

## Key JavaScript Concepts

- **`addEventListener('message', ...)`:** Receives messages from other browsing contexts.
- **`postMessage()`:** Sends messages between windows and frames.
- **`indexOf()`:** Searches for a substring but does not validate the URL scheme or destination.
- **`location.href`:** Navigates the browser to a specified URL and can execute JavaScript URLs in applicable contexts.
- **`javascript:` URL:** A URL scheme that executes JavaScript when navigated to in supported contexts.

## Security Impact

An attacker may exploit insecure web message handling and weak URL validation to execute JavaScript in the context of the vulnerable application.

Potential consequences include:

- DOM-based Cross-Site Scripting.
- Unauthorized actions performed through the victim's authenticated session.
- Manipulation of application content.
- Phishing through malicious page content.
- Access to information available to JavaScript in the application's context.

## Remediation

1. Validate incoming message origins using `event.origin`.
2. Validate the complete URL instead of checking for substrings using `indexOf()`.
3. Use the `URL` API to parse and validate URLs.
4. Allow only explicitly approved URL schemes, such as `https:`.
5. Avoid assigning untrusted data directly to `location.href`.
6. Reject JavaScript URLs and unexpected URL schemes.
7. Apply strict input validation to all incoming web messages.

## Evidence

<img width="1562" height="325" alt="image" src="https://github.com/user-attachments/assets/7cc1eb03-cce0-477b-86f5-01a735e06d61" />


## Reference

- [PortSwigger: DOM XSS Using Web Messages and a JavaScript URL](https://portswigger.net/web-security/dom-based/controlling-the-web-message-source/lab-dom-xss-using-web-messages-and-a-javascript-url)

