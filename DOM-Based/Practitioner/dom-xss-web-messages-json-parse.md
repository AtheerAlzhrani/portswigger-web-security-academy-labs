\# DOM XSS Using Web Messages and JSON.parse



| Field | Details |

|---|---|

| Platform | PortSwigger Web Security Academy |

| Difficulty | Practitioner |

| Category | DOM-Based XSS |

| Status | Solved |

| Vulnerability | DOM-based Cross-Site Scripting |

| Tools | Burp Suite, Browser DevTools, Exploit Server |



\## Objective



Exploit a DOM-based XSS vulnerability caused by insecure handling of web messages and JSON parsing to execute JavaScript and trigger the `print()` function.



\## Methodology



1\. Inspected the application's JavaScript and identified an event listener handling incoming web messages.

2\. Observed that incoming messages were processed using `JSON.parse()`.

3\. Identified a `switch` statement that checks the message's `type` property.

4\. Found that the `load-channel` case assigns the message's `url` property to an iframe's `src` attribute.

5\. Determined that the URL was not properly validated before being assigned.



\## Exploitation



Created an HTML payload on the exploit server containing an iframe that sends a crafted web message when loaded.



\*\*Payload:\*\*



```html

<iframe src="https://YOUR-LAB-ID.web-security-academy.net/"

onload='this.contentWindow.postMessage(

"{\\"type\\":\\"load-channel\\",\\"url\\":\\"javascript:print()\\"}","\*"

)'>

</iframe>

```



\*\*Execution steps:\*\*



1\. Replaced `YOUR-LAB-ID` with the target lab ID.

2\. Added the payload to the exploit server's HTML body.

3\. Stored the exploit.

4\. Delivered the exploit to the victim.

5\. The message was received, parsed using `JSON.parse()`, and processed by the event listener.

6\. The application assigned the JavaScript URL to the iframe's `src` attribute, triggering `print()`.



\## Vulnerability Analysis



The application trusted incoming web messages without properly validating their origin or the supplied URL.



The `load-channel` handler allowed an attacker to control an iframe's `src` attribute through a crafted JSON message.



This resulted in JavaScript execution in the application's context.



\## Impact



\- DOM-based Cross-Site Scripting (XSS).

\- Execution of attacker-controlled JavaScript.

\- Potential access to sensitive application data within the affected origin.

\- Possible manipulation of application functionality.



\## Remediation



\- Validate the origin of incoming messages using `event.origin`.

\- Validate the structure and expected values of incoming messages.

\- Restrict iframe URLs to trusted destinations.

\- Avoid assigning untrusted input to sensitive DOM properties.

\- Avoid using `javascript:` URLs.



\## Evidence



\- \[ ] Screenshot of the vulnerable message handler.

\- \[ ] Screenshot of the exploit server payload.

\- \[ ] Screenshot showing successful lab completion.

