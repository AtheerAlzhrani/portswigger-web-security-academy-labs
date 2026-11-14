# Lab: Reflected XSS with AngularJS Sandbox Escape and CSP

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Expert  
**Category:** Cross-Site Scripting (XSS) – AngularJS Sandbox Escape / CSP Bypass  
**Status:** Solved  
**Tools:** Burp Suite, Browser, PortSwigger Exploit Server

## Objective

Exploit a reflected XSS vulnerability in an AngularJS application by bypassing the Content Security Policy (CSP), escaping the AngularJS sandbox, and executing JavaScript to retrieve `document.cookie`.

## Methodology

1. Examined the lab and identified that it uses AngularJS together with a Content Security Policy.
2. Prepared an exploit using the PortSwigger Exploit Server.
3. Used the AngularJS `ng-focus` directive to trigger an event when the injected input element receives focus.
4. Accessed the event object through AngularJS's `$event` variable.
5. Used `$event.composedPath()` to retrieve the event's propagation path, which includes the `window` object.
6. Used the AngularJS `orderBy` filter to process the event path.
7. Assigned the `alert` function to a variable and invoked it when the filter reached the appropriate execution context.
8. Stored the exploit and selected **Deliver exploit to victim**.
9. Confirmed successful execution when the victim's browser displayed the cookie value.

## Exploit Payload

The following code was submitted to the Exploit Server, replacing the lab ID with the target lab identifier:

```html
<script>
location='https://YOUR-LAB-ID.web-security-academy.net/?search=%3Cinput%20id=x%20ng-focus=$event.composedPath()|orderBy:%27(z=alert)(document.cookie)%27%3E#x';
</script>
```

## Technical Analysis

### 1. Triggering AngularJS event execution

```html
<input id=x ng-focus=...>
```

The `ng-focus` directive executes its AngularJS expression when the input receives focus.

The `#x` URL fragment targets the injected element, triggering the focus event when the page loads.

### 2. Accessing the event propagation path

```javascript
$event.composedPath()
```

The `$event` variable provides access to the current event object.

`composedPath()` returns the sequence of objects through which the event propagates. In this lab's browser environment, the resulting path includes the `window` object.

### 3. Using the AngularJS orderBy filter

```javascript
$event.composedPath() | orderBy:'(z=alert)(document.cookie)'
```

The pipe character invokes the AngularJS filter mechanism, and the colon supplies the filter argument.

The expression assigns `alert` to the variable `z` and invokes it with `document.cookie` as its argument.

### 4. Bypassing the AngularJS sandbox

AngularJS applies restrictions to expressions that reference sensitive global objects such as `window`.

The exploit uses the event propagation path to reach the `window` object indirectly. When the filter processes the relevant object, the expression executes in the required context without explicitly referencing `window`.

### 5. CSP bypass

The exploit uses AngularJS's event-handling functionality rather than relying on a conventional injected inline JavaScript event handler or a directly injected script in the vulnerable page.

This allows the exploit to execute despite the lab's CSP restrictions.

## Vulnerability

The application reflects user-controlled input into an AngularJS expression context without safely preventing attacker-controlled expression execution.

The combination of AngularJS expression evaluation, event handling, and an exploitable sandbox implementation allows an attacker to bypass the sandbox and execute JavaScript despite the CSP restrictions.

## Impact

Successful exploitation can allow an attacker to execute JavaScript in a victim's browser under the vulnerable application's origin.

Potential consequences include:

- Access to sensitive information exposed to JavaScript.
- Unauthorized actions using the victim's authenticated session.
- Manipulation of application content.
- Exposure of accessible cookie values.
- Further client-side attacks.

## Remediation

- Avoid compiling or evaluating untrusted user input as AngularJS expressions.
- Do not rely on AngularJS sandbox restrictions as a security boundary.
- Apply context-aware output encoding.
- Use safe DOM APIs when handling user-controlled content.
- Configure CSP as defense in depth rather than relying on it as the sole XSS protection.
- Migrate from AngularJS to a supported framework.

## Evidence

- [ ] Screenshot of the Exploit Server payload.
- [ ] Screenshot showing the exploit delivery.
- [ ] Screenshot of the successful `document.cookie` alert.

## Lab Reference

[PortSwigger – AngularJS sandbox escape and CSP](https://portswigger.net/web-security/cross-site-scripting/contexts/client-side-template-injection/lab-angular-sandbox-escape-and-csp)

