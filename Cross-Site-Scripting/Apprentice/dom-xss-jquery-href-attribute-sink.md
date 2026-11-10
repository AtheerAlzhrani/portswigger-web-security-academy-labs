# DOM XSS in jQuery href Attribute Sink

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Apprentice  
**Category:** Cross-Site Scripting (DOM-based XSS)  
**Status:** Solved  
**Tool(s):** Firefox

## Objective

Exploit a DOM-based XSS vulnerability in the Submit Feedback page by manipulating the `returnPath` query parameter to make the Back link execute JavaScript that displays `document.cookie`.

## Methodology

1. Opened the Submit Feedback page.
2. Modified the `returnPath` query parameter to contain `/` followed by a random alphanumeric string.
3. Inspected the Back link using Browser Developer Tools.
4. Observed that the supplied value was inserted into the anchor element's `href` attribute.
5. Replaced the `returnPath` value with the following JavaScript URL:

## Payload

```javascript
javascript:alert(document.cookie)
```

6. Pressed Enter to load the modified URL.
7. Clicked the Back link.
8. The browser executed the JavaScript URL and displayed the document's cookies, successfully solving the lab.

## Vulnerability

The application uses jQuery to select an anchor element and update its `href` attribute using data from `location.search`.

The application does not properly validate the URL scheme before assigning the value to the `href` attribute.

As a result, an attacker can supply a `javascript:` URL instead of a normal navigation URL. When a user clicks the Back link, the browser executes the JavaScript.

**Source:** `location.search`  
**Sink:** jQuery-based `href` attribute assignment  
**Affected parameter:** `returnPath`  
**Injection context:** URL attribute  
**Attack vector:** JavaScript URL (`javascript:`)

## Impact

Successful exploitation could allow an attacker to execute JavaScript in the context of the vulnerable website.

Potential consequences include:
- Access to cookies accessible through JavaScript.
- Unauthorized actions performed in the victim's session.
- Access to sensitive information available to client-side JavaScript.
- Manipulation of page content.

Note: Cookies protected with the `HttpOnly` flag cannot be accessed through `document.cookie`.

## Remediation

- Validate URL values before assigning them to `href` attributes.
- Allow only trusted URL schemes, such as `https:` and `http:`, where appropriate.
- Reject dangerous schemes such as `javascript:`.
- Avoid directly assigning untrusted input to DOM attributes.
- Implement a restrictive Content Security Policy (CSP) as an additional security layer.

## Evidence

<img width="1286" height="1042" alt="image" src="https://github.com/user-attachments/assets/091bd06f-b0d2-4a04-afa1-01593c8d6933" />

<img width="1362" height="772" alt="image" src="https://github.com/user-attachments/assets/aecfd23f-68df-4ede-ae78-2ef7b19b6224" />


