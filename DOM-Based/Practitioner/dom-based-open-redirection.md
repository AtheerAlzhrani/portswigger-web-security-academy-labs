# Lab: DOM-Based Open Redirection

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Practitioner  
**Category:** DOM-Based Vulnerabilities  
**Status:** Solved  
**Tools:** Firefox, PortSwigger Web Security Academy

## Objective

Exploit a DOM-based open redirection vulnerability by manipulating the `url` parameter to redirect users to an attacker-controlled destination.

## Vulnerability Overview

The blog post page contains a **Back to Blog** link that uses JavaScript to extract a URL from the current page's address.

The JavaScript uses a regular expression to identify the `url` parameter and assigns the extracted value to `location.href`.

Because the application does not validate the destination URL, an attacker can manipulate the `url` parameter to redirect users to an arbitrary external website.

## Methodology

1. Inspected the blog post page using browser DevTools.
2. Identified the **Back to Blog** link and its JavaScript click handler.
3. Observed that the handler extracts the destination URL from the page's address using a regular expression.
4. Identified that the extracted URL is assigned to `location.href`.
5. Constructed a URL containing the exploit server as the redirect destination.
6. Visited the crafted URL.
7. Clicked the **Back to Blog** link.
8. Successfully redirected to the exploit server, completing the lab.

## Payload

```url
https://YOUR-LAB-ID.web-security-academy.net/post?postId=4&url=https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net/
```

## Technical Explanation

The exploit works through the following sequence:

1. The crafted URL loads the specified blog post.
2. The `url` parameter contains the attacker-controlled redirect destination.
3. The JavaScript click handler on the **Back to Blog** link extracts the URL using a regular expression.
4. The extracted destination is assigned to `location.href`.
5. When the user clicks the link, the browser navigates to the supplied destination.
6. The browser redirects to the exploit server.

### Key JavaScript Concepts

- `location`: Provides access to the current browser URL.
- `RegExp`: Used to extract the redirect destination from the URL.
- `location.href`: Navigates the browser to a specified URL.
- URL parameters: Allow data to be passed through the query string.

## Security Impact

An attacker may exploit an open redirection vulnerability to redirect users to attacker-controlled websites.

Depending on the application's context, this could potentially lead to:

- Phishing attacks.
- Abuse of trusted application links.
- Redirecting users to malicious websites.
- Increased credibility of malicious links using a trusted domain.

## Remediation

1. Avoid extracting redirect destinations directly from user-controlled URL parameters.
2. Validate redirect destinations against an allowlist of trusted domains.
3. Avoid unrestricted assignments to `location.href`.
4. Prefer predefined internal redirect destinations instead of accepting arbitrary URLs.
5. Reject unexpected or untrusted redirect destinations.

## Evidence

<img width="1647" height="464" alt="image" src="https://github.com/user-attachments/assets/78255f36-3130-4eb5-ba2e-6690cfc0d809" />


## Reference

[PortSwigger: DOM-Based Open Redirection](https://portswigger.net/web-security/dom-based/open-redirection/lab-dom-open-redirection)
