# DOM XSS in AngularJS Expression with Angle Brackets and Double Quotes HTML-Encoded

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Practitioner  
**Category:** DOM-Based Cross-Site Scripting (XSS)  
**Status:** Solved  
**Tool:** Firefox

## Objective

Exploit a DOM-based XSS vulnerability in an AngularJS application by injecting a malicious AngularJS expression into a page where user-controlled input is processed by an `ng-app` directive.

## Methodology

1. Entered a random alphanumeric string into the website's search functionality.
2. Inspected the page source and observed that the reflected input was enclosed within an AngularJS `ng-app` directive.
3. Identified that the application processed the user-controlled input as an AngularJS expression.
4. Replaced the original search input with a crafted AngularJS expression.
5. Submitted the payload using the Search button.
6. Successfully triggered a JavaScript alert, confirming the DOM-based XSS vulnerability.

## Payload

```javascript
{{$on.constructor('alert(1)')()}}
```

## Vulnerability

The application reflected user-controlled input inside an AngularJS application context.

Because the input was processed as an AngularJS expression, an attacker could inject an expression that accessed JavaScript functionality through the `$on` object's constructor.

The injected expression used the `constructor` property to create and execute a JavaScript function containing `alert(1)`.

This allowed JavaScript execution despite angle brackets and double quotes being HTML-encoded.

## Impact

An attacker could potentially:

- Execute arbitrary JavaScript in a victim's browser.
- Manipulate page content and application behavior.
- Perform unauthorized actions within the victim's session.
- Access sensitive information exposed to client-side JavaScript.
- Conduct phishing attacks.

## Remediation

- Never compile or evaluate untrusted user input as AngularJS expressions.
- Avoid dynamically inserting user-controlled content into AngularJS templates.
- Use safe text-binding mechanisms instead of evaluating expressions.
- Keep AngularJS and its dependencies updated.
- Apply appropriate input validation and output encoding.
- Implement a restrictive Content Security Policy (CSP) as an additional security layer.

## Evidence
<img width="1205" height="600" alt="image" src="https://github.com/user-attachments/assets/0aba7d3f-caef-4494-849d-644a58982776" />
<img width="1157" height="317" alt="image" src="https://github.com/user-attachments/assets/94e72886-1ecc-4b45-b4aa-9adcf4dbb2f3" />

<img width="586" height="330" alt="image" src="https://github.com/user-attachments/assets/1b8b8c6f-0666-4398-91c5-fb09fa86abb8" />
<img width="1552" height="272" alt="image" src="https://github.com/user-attachments/assets/51e04b9e-4891-4169-ad82-4495bc90ccf4" />



## Lab Reference

[PortSwigger Web Security Academy – DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded.](https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-angularjs-expression)
