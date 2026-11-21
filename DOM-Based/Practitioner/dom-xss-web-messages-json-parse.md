# Lab: DOM XSS Using Web Messages and `JSON.parse`

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Practitioner  
**Category:** DOM-Based Vulnerabilities  
**Status:** Solved  
**Tools:** Firefox, PortSwigger Exploit Server

## Objective

Exploit a DOM-based Cross-Site Scripting (DOM XSS) vulnerability by sending a crafted JSON web message to the target application, causing the browser to execute the JavaScript `print()` function.

## Vulnerability Overview

The application contains a JavaScript event listener that listens for incoming messages using `window.addEventListener('message', ...)`.

The received message is parsed using `JSON.parse()` and processed through a `switch` statement based on its `type` property.

The `load-channel` case assigns the message's `url` property to the `src` attribute of an iframe.

Because the application does not properly validate the supplied URL, an attacker can provide a malicious JavaScript URL through a crafted web message.

This allows JavaScript execution in the context of the target application.

## Methodology

1. Inspected the application's homepage using browser DevTools.
2. Identified a JavaScript `addEventListener()` function listening for incoming web messages.
3. Observed that incoming messages are processed using `JSON.parse()`.
4. Identified the `load-channel` case in the `switch` statement.
5. Discovered that the application assigns the message's `url` property to an iframe's `src` attribute.
6. Opened the PortSwigger Exploit Server.
7. Created an iframe pointing to the vulnerable target application.
8. Used the iframe's `onload` event to send a crafted JSON message using `postMessage()`.
9. Stored the exploit and delivered it to the victim.
10. Successfully triggered the `print()` function.

## Payload

```html
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/"
onload='this.contentWindow.postMessage(
"{\"type\":\"load-channel\",\"url\":\"javascript:print()\"}","*"
)'>
</iframe>
```

## Technical Explanation

The exploit works through the following sequence:

1. The iframe loads the vulnerable application.
2. The `onload` event executes after the iframe finishes loading.
3. `postMessage()` sends a crafted JSON message to the target window.
4. The target application's message event listener receives the message.
5. `JSON.parse()` converts the message string into a JavaScript object.
6. The `switch` statement processes the `load-channel` message type.
7. The application assigns the supplied `url` value to the iframe's `src` attribute.
8. The browser executes the injected JavaScript URL, triggering `print()`.

### Key JavaScript Concepts

- `window.addEventListener('message', ...)`: Receives messages from other windows.
- `postMessage()`: Allows communication between different browsing contexts.
- `event.data`: Contains the received message data.
- `JSON.parse()`: Converts a JSON string into a JavaScript object.
- `switch`: Executes logic based on the message's `type` property.
- `iframe.src`: Sets the URL loaded inside an iframe.
- `javascript:` URL: Executes JavaScript when assigned to a supported navigation context.

## Security Impact

An attacker may exploit insecure web message handling to execute JavaScript in the context of the vulnerable application.

Depending on the application's functionality and the victim's privileges, this could potentially lead to:

- Unauthorized actions performed through the victim's authenticated session.
- Manipulation of application content.
- Access to sensitive information exposed to JavaScript.
- Execution of attacker-controlled scripts.

## Remediation

1. Validate incoming messages before processing them.
2. Verify the sender's origin using `event.origin`.
3. Validate message properties and expected data types.
4. Restrict iframe URLs to trusted destinations.
5. Reject dangerous URL schemes such as `javascript:`.
6. Avoid assigning untrusted input to sensitive DOM properties.
7. Implement a strict Content Security Policy (CSP) as an additional security layer.

## Evidence
<img width="1622" height="579" alt="image" src="https://github.com/user-attachments/assets/1e19bf54-f9d4-455a-8354-1091924cb210" />


## Reference

https://portswigger.net/web-security/dom-based/controlling-the-web-message-source/lab-dom-xss-using-web-messages-and-json-parse
