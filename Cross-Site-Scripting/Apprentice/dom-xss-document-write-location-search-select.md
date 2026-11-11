# DOM XSS in document.write Sink Using location.search Inside a Select Element

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Apprentice  
**Category:** DOM-Based Cross-Site Scripting (XSS)  
**Status:** Solved  
**Tools:** Burp Suite, Browser Developer Tools

## Objective

Exploit a DOM-based XSS vulnerability caused by unsafe use of `document.write()` with attacker-controlled data obtained from the `location.search` URL source.

## Methodology

1. Inspected the JavaScript functionality on the product pages.
2. Identified that the application extracted the `storeId` parameter from `location.search`.
3. Observed that the extracted value was passed directly to `document.write()`.
4. Added a `storeId` query parameter containing a random alphanumeric string.
5. Loaded the modified URL and confirmed that the random string appeared as an option in the stock checker's dropdown list.
6. Inspected the dropdown list using browser Developer Tools and confirmed that the input was inserted inside a `<select>` element.
7. Modified the `storeId` parameter with an XSS payload to escape the select element and inject an HTML element with an event handler.
8. Loaded the modified URL and successfully triggered a JavaScript alert.

## Payload

```html
"></select><img src=1 onerror=alert(1)>
```

**Source:** `location.search`  
**Sink:** `document.write()`  
**Injection Context:** HTML inside a select element.

## Vulnerability

The application extracted user-controlled input from the URL using `location.search` and inserted it directly into the page using `document.write()`.

The application did not properly sanitize or encode the input before inserting it into the HTML document.

By injecting a closing `</select>` tag, it was possible to escape the existing HTML context and introduce an image element.

The image used an invalid source (`src=1`), causing its `onerror` event handler to execute the injected JavaScript.

## Impact

An attacker could potentially:

- Execute arbitrary JavaScript in a victim's browser.
- Manipulate the application's DOM and page content.
- Perform unauthorized actions within the victim's session.
- Access sensitive information exposed to client-side JavaScript.
- Conduct phishing attacks.

## Remediation

- Avoid using `document.write()` with untrusted input.
- Use safer DOM APIs such as `textContent` when inserting plain text.
- Validate and sanitize URL parameters before processing them.
- Apply context-aware output encoding.
- Implement a restrictive Content Security Policy (CSP) as an additional security layer.

## Evidence

- [ ] Screenshot showing the random `storeId` value in the dropdown.
- [ ] Screenshot of the vulnerable JavaScript code.
- [ ] Screenshot of the injected payload in the URL.
- [ ] Screenshot of the successful JavaScript alert.

## Lab Reference

PortSwigger Web Security Academy – DOM XSS in `document.write` sink using source `location.search` inside a select element.
