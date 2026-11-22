\# Lab: Clobbering DOM Attributes to Bypass HTML Filters



\*\*Platform:\*\* PortSwigger Web Security Academy  

\*\*Difficulty:\*\* Expert  

\*\*Category:\*\* DOM-Based Vulnerabilities (DOM Clobbering)  

\*\*Status:\*\* Solved  

\*\*Tools:\*\* Chrome, PortSwigger Exploit Server



\## Objective



Exploit a DOM-clobbering vulnerability in the HTMLJanitor library to bypass HTML attribute filtering and inject an event handler that executes the JavaScript `print()` function.



\## Vulnerability Overview



The application uses the HTMLJanitor library to sanitize user-submitted HTML comments.



The library relies on the `attributes` property to inspect and filter HTML attributes. However, this property can be manipulated through DOM clobbering.



By injecting HTML elements that clobber the `attributes` property, an attacker can interfere with the library's filtering logic and bypass its attribute restrictions.



This allows an attacker to inject an event handler into an HTML element and trigger JavaScript execution.



\## Methodology



1\. Inspected the blog post and comment functionality.

2\. Identified that user-submitted HTML is processed using the HTMLJanitor library.

3\. Investigated how the library filters HTML attributes.

4\. Identified that the `attributes` property could be clobbered through injected HTML.

5\. Submitted a crafted HTML comment containing a form and input element to interfere with attribute filtering.

6\. Opened the PortSwigger Exploit Server.

7\. Created an iframe targeting the blog post containing the injected comment.

8\. Used a delayed URL fragment change to focus the clobbered element.

9\. Stored the exploit and delivered it to the victim.

10\. Successfully triggered the `print()` function.



\## Payload



\*\*Comment payload:\*\*



```html

<form id=x tabindex=0 onfocus=print()><input id=attributes>

```



\*\*Exploit Server payload:\*\*



```html

<iframe src=https://YOUR-LAB-ID.web-security-academy.net/post?postId=3 onload="setTimeout(()=>this.src=this.src+'#x',500)">

</iframe>

```



Replace `YOUR-LAB-ID` with your lab ID and `postId=3` with the ID of the blog post containing your injected comment.



\## Technical Explanation



The exploit works through the following sequence:



1\. The injected form and input elements interfere with the DOM-based handling of the `attributes` property.

2\. DOM clobbering causes the HTMLJanitor filtering logic to behave unexpectedly.

3\. The attribute filtering mechanism is bypassed, allowing the `onfocus` attribute to remain on the form.

4\. The exploit server loads the blog post containing the injected comment inside an iframe.

5\. After a 500ms delay, the iframe navigates to the same URL with the `#x` fragment appended.

6\. The fragment directs the browser to the element identified by `id=x`.

7\. The browser focuses the form element.

8\. The `onfocus` handler executes `print()`.



\### Key JavaScript and HTML Concepts



\- \*\*DOM Clobbering:\*\* Manipulating DOM elements to interfere with JavaScript properties and application logic.

\- `attributes`: DOM property used to access an element's attributes.

\- `form`: HTML element used in the clobbering payload.

\- `tabindex`: Makes an element focusable.

\- `onfocus`: Event handler triggered when an element receives focus.

\- `id=x`: Identifies the element targeted by the URL fragment.

\- URL fragments (`#x`): Can navigate to or focus an element identified by its ID.

\- `setTimeout()`: Delays execution to allow the page and injected content to load.



\## Security Impact



An attacker may exploit DOM clobbering to bypass HTML sanitization and inject malicious attributes.



Depending on the application's functionality and the victim's privileges, this could potentially lead to:



\- DOM-based Cross-Site Scripting (XSS).

\- Bypassing HTML sanitization mechanisms.

\- Execution of attacker-controlled JavaScript.

\- Manipulation of application functionality.



\## Remediation



1\. Use well-maintained HTML sanitization libraries and keep them updated.

2\. Avoid relying on DOM properties that can be clobbered by injected elements.

3\. Validate and sanitize user-submitted HTML using robust allowlist-based rules.

4\. Prevent unsafe event-handler attributes such as `onfocus` from being retained.

5\. Avoid unsafe DOM manipulation patterns.

6\. Implement a strict Content Security Policy (CSP) as an additional security layer.



\## Evidence



\_Add screenshot showing successful lab completion.\_



\## Reference



\[PortSwigger: Clobbering DOM Attributes to Bypass HTML Filters](https://portswigger.net/web-security/dom-based/dom-clobbering/lab-dom-clobbering-attributes-to-bypass-html-filters)

