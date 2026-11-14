# Lab: Reflected XSS Protected by CSP, with CSP Bypass

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Expert  
**Category:** Cross-Site Scripting (XSS) – Content Security Policy (CSP) Bypass  
**Status:** Solved  
**Tools:** Burp Suite ,Firefox

## Objective

Exploit a reflected XSS vulnerability by bypassing the application's Content Security Policy and executing JavaScript using `alert(1)`.

## Methodology

1. Submitted the following payload through the search functionality:

   ```html
   <img src=1 onerror=alert(1)>
   ```

2. Observed that the payload was reflected in the response, but the browser's Content Security Policy prevented JavaScript execution.
3. Inspected the HTTP response in Burp Suite Proxy.
4. Identified the `Content-Security-Policy` response header and examined its `report-uri` directive.
5. Discovered that the `token` parameter used in the CSP configuration was controllable.
6. Tested whether the parameter could be manipulated to inject an additional CSP directive.
7. Injected the following directive:

   ```text
   ;script-src-elem 'unsafe-inline'
   ```

8. Combined the CSP injection with the reflected XSS payload.
9. Loaded the crafted URL in Chrome and confirmed that the injected JavaScript executed.

## Exploit Payload

**Reflected XSS:**

```html
<script>alert(1)</script>
```

**CSP injection:**

```text
;script-src-elem 'unsafe-inline'
```

**Full URL structure:**

```text
https://YOUR-LAB-ID.web-security-academy.net/?search=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&token=;script-src-elem%20%27unsafe-inline%27
```

## Technical Analysis

### 1. Identifying the CSP restriction

The application uses a Content Security Policy to restrict JavaScript execution.

Although the reflected XSS payload was present in the response, the browser blocked its execution because of the configured CSP.

### 2. Identifying the injection point

Inspection of the response revealed a `report-uri` directive containing a `token` parameter.

Because this parameter was controllable, it provided an opportunity to inject additional CSP directives.

### 3. Injecting a new CSP directive

```text
;script-src-elem 'unsafe-inline'
```

The semicolon separates CSP directives.

The injected `script-src-elem` directive controls which sources are permitted for JavaScript elements such as `<script>` tags.

The `'unsafe-inline'` source expression allows inline script elements to execute.

### 4. Executing the reflected XSS

```html
<script>alert(1)</script>
```

After the CSP injection, the browser accepts the inline script and executes the alert.

**Browser requirement:** The intended solution works in Google Chrome.

## Vulnerability

The application contains two security weaknesses:

1. A reflected XSS vulnerability that allows attacker-controlled HTML and JavaScript to appear in the response.
2. A CSP injection vulnerability caused by incorporating an untrusted parameter into the Content Security Policy configuration without proper validation.

The second vulnerability allows an attacker to modify the security policy and weaken its restrictions, enabling exploitation of the reflected XSS.

## Impact

Successful exploitation can allow an attacker to execute arbitrary JavaScript in a victim's browser under the vulnerable application's origin.

Potential consequences include:

- Unauthorized actions through the victim's authenticated session.
- Access to information available to client-side JavaScript.
- Manipulation of application content.
- Further client-side attacks.

## Remediation

- Never construct CSP directives using untrusted user input.
- Validate and safely encode values used in CSP configuration.
- Define CSP policies using trusted, server-controlled configuration.
- Avoid allowing `'unsafe-inline'` in script directives.
- Use nonces or hashes for authorized inline scripts.
- Fix the underlying reflected XSS vulnerability through context-aware output encoding.
- Test CSP headers to ensure injected parameters cannot modify policy directives.

## Evidence

<img width="1487" height="447" alt="image" src="https://github.com/user-attachments/assets/b09b0e2a-b013-47ea-b4b7-034a28ecdf54" />

<img width="1612" height="676" alt="image" src="https://github.com/user-attachments/assets/1a3725c0-bf1a-4930-8c68-cdf23903ace9" />
<img width="1581" height="280" alt="image" src="https://github.com/user-attachments/assets/ecf10fc2-3b61-44a7-9d83-0d4cb277a76f" />




## Lab Reference
https://portswigger.net/web-security/cross-site-scripting/content-security-policy/lab-csp-bypass
