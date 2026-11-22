# Lab: Exploiting DOM Clobbering to Enable XSS

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Expert  
**Category:** DOM-Based Vulnerabilities (DOM Clobbering)  
**Status:** Solved  
**Tools:** Firefox, PortSwigger Web Security Academy

## Objective

Exploit a DOM-clobbering vulnerability by injecting HTML anchors that overwrite a JavaScript global variable, resulting in JavaScript execution and triggering the `alert()` function.

## Vulnerability Overview

The application contains a JavaScript implementation that initializes a `defaultAvatar` variable using a global DOM property:

```javascript
let defaultAvatar = window.defaultAvatar || {
    avatar: '/resources/images/avatarDefault.svg'
};
```

Because the application relies on a global property that can be influenced by HTML elements in the DOM, an attacker can manipulate the variable through DOM clobbering.

The comment functionality allows certain HTML elements, including anchor tags, to be injected into the page. By creating anchors with matching IDs and carefully crafted attributes, an attacker can clobber the `defaultAvatar` object and control its `avatar` property.

When the page subsequently processes comments, the manipulated value is used in a context that allows JavaScript execution.

## Methodology

1. Inspected the blog post page and its JavaScript implementation.
2. Identified the `defaultAvatar` variable and its initialization using `window.defaultAvatar`.
3. Observed that the comment functionality permits certain HTML elements, including anchor tags.
4. Identified a DOM-clobbering opportunity through elements with matching IDs and named properties.
5. Submitted a crafted comment containing two anchor elements designed to clobber the `defaultAvatar` variable.
6. Submitted a second comment containing arbitrary text.
7. Reloaded the blog post.
8. Successfully triggered the `alert()` function.

## Payload

```html
<a id=defaultAvatar><a id=defaultAvatar name=avatar href="cid:&quot;onerror=alert(1)//">
```

## Technical Explanation

The exploit works through the following sequence:

1. The first anchor creates a named DOM element through the `defaultAvatar` ID.
2. The second anchor uses the same ID, causing the elements to be exposed as a DOM collection.
3. The `name=avatar` attribute creates a named property within the collection.
4. The `href` attribute supplies the manipulated value.
5. The application's JavaScript retrieves the clobbered `defaultAvatar` global instead of using its fallback object.
6. The manipulated `avatar` value is subsequently processed by the application.
7. The injected `onerror` handler is triggered, executing `alert()`.

### Key JavaScript Concepts

- **DOM Clobbering:** Manipulating named DOM elements to interfere with JavaScript global variables and object properties.
- `window.defaultAvatar`: A global property that can be influenced by named DOM elements.
- `id`: Can expose elements as named properties on the global window object.
- `name`: Can expose named properties within certain DOM collections.
- `href`: Supplies the crafted value used in the clobbering chain.
- `onerror`: Executes JavaScript when an associated resource fails to load.

## Security Impact

An attacker may exploit DOM clobbering to manipulate JavaScript variables and influence application behavior.

Depending on the application's functionality, this could potentially lead to:

- DOM-based Cross-Site Scripting (XSS).
- Execution of attacker-controlled JavaScript.
- Manipulation of application logic.
- Potential access to sensitive information exposed to JavaScript.

## Remediation

1. Avoid relying on DOM-exposed global variables for security-sensitive logic.
2. Use explicit variable initialization instead of relying on `window` properties influenced by DOM elements.
3. Avoid inserting untrusted HTML into the DOM.
4. Sanitize user-generated HTML using a well-maintained sanitizer.
5. Avoid unsafe HTML and attribute handling.
6. Use a strict Content Security Policy (CSP) as an additional security layer.

## Evidence

<img width="1571" height="272" alt="image" src="https://github.com/user-attachments/assets/33d5c4ff-e5fb-441f-8702-6bd340ea030a" />


## Reference

[PortSwigger: Exploiting DOM Clobbering to Enable XSS](https://portswigger.net/web-security/dom-based/dom-clobbering/lab-dom-xss-exploiting-dom-clobbering)
