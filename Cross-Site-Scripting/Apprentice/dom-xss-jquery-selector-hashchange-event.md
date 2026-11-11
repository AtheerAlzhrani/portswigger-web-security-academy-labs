# DOM XSS in jQuery Selector Sink Using a Hashchange Event

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Apprentice  
**Category:** Cross-Site Scripting (DOM-based XSS)  
**Status:** Solved  
**Tools:** Firefox

## Objective

Exploit a DOM-based XSS vulnerability on the home page by manipulating the URL fragment and delivering an exploit to the victim that executes the `print()` function.

## Methodology

1. Inspected the home page JavaScript using Browser Developer Tools.
2. Identified that the application uses jQuery's `$()` selector function inside a `hashchange` event handler.
3. Observed that the application retrieves data from `location.hash` and uses it to construct a jQuery selector.
4. Opened the Exploit Server from the lab banner.
5. Added an iframe payload to the Body section, pointing to the vulnerable lab.
6. Used the iframe's `onload` event to append an HTML payload to the URL fragment, triggering the `hashchange` event.
7. Stored the exploit and selected **View exploit** to verify that `print()` executed.
8. Selected **Deliver exploit to victim** to successfully solve the lab.

## Payload

```html
<iframe src="https://0afb00e804c8530080f9e96d00030086.web-security-academy.net/#" onload="this.src+='<img src=x onerror=print()>'"></iframe>
```

## Vulnerability

The application uses jQuery's `$()` selector function to locate blog post headings based on the URL fragment.

The `location.hash` value is decoded and passed into the jQuery selector without appropriate validation or safe handling.

This allows attacker-controlled input to be interpreted as HTML through the vulnerable selector.

**Source:** `location.hash`  
**Sink:** jQuery `$()` selector function  
**Event:** `hashchange`  
**Injection context:** jQuery selector  
**Execution:** `print()` through an injected image's `onerror` event.

## Exploit Explanation

The payload works in the following stages:

1. The iframe loads the vulnerable application with an empty URL fragment (`#`).
2. When the iframe finishes loading, its `onload` event appends the malicious HTML payload to the URL.
3. This changes the URL fragment and triggers the `hashchange` event.
4. The vulnerable JavaScript processes the fragment using jQuery's `$()` selector.
5. The injected image has an invalid source, causing its `onerror` event to execute.
6. The `print()` function is called in the victim's browser.

## Impact

Successful exploitation could allow an attacker to execute JavaScript in a victim's browser within the vulnerable application's origin.

Potential consequences include:
- Unauthorized actions performed through the victim's session.
- Access to information available to client-side JavaScript.
- Manipulation of page content.
- Client-side phishing attacks.

## Remediation

- Avoid passing untrusted input directly into jQuery selectors.
- Use safer DOM APIs instead of constructing selectors from user-controlled data.
- Validate and safely handle URL fragment values.
- Update vulnerable JavaScript libraries to secure versions.
- Implement a restrictive Content Security Policy (CSP) as an additional security layer.

## Evidence

<img width="1012" height="172" alt="image" src="https://github.com/user-attachments/assets/9a8b2535-7e3a-4e41-8c2c-79f62284505c" />
<img width="1572" height="981" alt="image" src="https://github.com/user-attachments/assets/3c1962da-cb81-4383-9b00-63dce78b745a" />

<img width="1632" height="1015" alt="image" src="https://github.com/user-attachments/assets/abc70171-9295-4683-98ad-3a317befedf4" />
<img width="1541" height="277" alt="image" src="https://github.com/user-attachments/assets/e9cee8a2-c710-4594-a6b7-f3bea25ef55d" />
