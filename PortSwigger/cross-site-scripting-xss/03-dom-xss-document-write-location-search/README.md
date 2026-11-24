# DOM XSS in document.write sink using source location.search

**Platform:** PortSwigger Web Security Academy  
**Difficulty:** Apprentice  
**Date Completed:** 24 November 2026  
**Status:** Solved

---

## Lab Description

This lab contains a DOM-based cross-site scripting vulnerability.  
The application uses `document.write` to write data from `location.search` (the query string) directly into the page.

The goal is to inject a payload via the URL that executes `alert(1)`.

---

## Tools Used

- Burp Suite Community Edition (Proxy + HTTP history)
- Firefox with FoxyProxy

---

## Solution Steps

### 1. Understand the sink

The vulnerable code roughly looks like this:

```javascript
document.write(.... location.search ....);
```

The value of `location.search` (everything after `?` in the URL) is written into the page without proper sanitization.

### 2. Craft the payload

Because the input is placed inside an existing HTML structure, we need to break out of it.  
A working payload is:

```html
"><svg onload=alert(1)>
```

### 3. Trigger the vulnerability

Enter the payload in the search box or directly in the URL:

```
/?search="><svg onload=alert(1)>
```

After submission the alert appears and the lab is marked as solved.

**Request observed in Burp:**

```http
GET /?search=%22%3E%3Csvg+onload%3Dalert%281%29%3E HTTP/2
```

---

## Why the Payload Works

- `location.search` contains the attacker's input.
- The application passes this value into `document.write` without encoding.
- The payload `"><svg onload=alert(1)>` closes the current attribute/tag and injects an SVG element with an `onload` event handler.
- When the browser parses the newly written HTML, the `onload` event fires and executes `alert(1)`.

Using an SVG with `onload` is a common technique when `<script>` tags are filtered or when you need to break out of an attribute context.

---

## Screenshots

### Lab solved

![Lab solved - DOM XSS](site-2.png)

### Request in Burp HTTP history

![Burp history showing the payload](bs-2.png)

---

## Key Takeaways

- DOM XSS occurs entirely on the client side — the server may not even see the final malicious HTML.
- Sources such as `location.search`, `location.hash`, `document.referrer` etc. are common.
- Sinks such as `document.write`, `innerHTML`, `eval`, etc. are dangerous when they process attacker-controlled data.
- When the input lands inside an existing tag/attribute, you often need to close it first (`">` or `'>`).
- Alternative event handlers (`onload`, `onerror`, `onfocus`…) are useful when classic `<script>` is blocked.

---

## References

- [PortSwigger – DOM-based XSS](https://portswigger.net/web-security/cross-site-scripting/dom-based)
- [Lab link](https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-document-write-sink)
