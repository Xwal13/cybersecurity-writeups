---
title: "Stored XSS — `href` attribute (double-quotes HTML-encoded)"
date: 2025-09-20
layout: post
categories: [security, xss, pentest]
tags: [xss, burp, writeup, vulnerability]
summary: "Stored XSS in comment 'Website' field — payload placed into an anchor `href`. Reproduction steps using Burp (Proxy + Repeater)."
---

# Stored XSS — `href` attribute (double-quotes HTML-encoded)

**Scope:** Comment form **Website** (author URL) → rendered into an anchor `href`.  
**Objective:** Demonstrate stored XSS by making the comment author link execute JavaScript when clicked (lab-only verification).

> **Important:** All payloads and HTML are shown as code. Do not execute against unauthorized systems. Test only in authorized / lab environments.

---

## Affected resource
- **Input:** Comment form → **Website (author URL)** field  
- **Sink:** Rendered into `<a href="USER_INPUT">Author</a>` for each comment author  
- **Encoding:** Double-quotes are HTML-encoded (`"` → `&quot;`) but the attribute **value** is not validated for URI scheme (allows `javascript:`)

---

## Exact payload (for verification)
```text
javascript:alert(1)
```

---

## Site reproduction steps (precise)
1. Open the target post page with the comment form.  
2. Fill the form:
   - **Name:** `attacker` (or any identifier)  
   - **Website:** ``javascript:alert(1)``  (payload as code; do not click)  
   - **Comment:** `test`  
3. Submit the comment.  
4. View the post where your comment appears. The author name is a link. Right-click the author name → **Copy link address** and inspect; it should contain `javascript:alert(1)`.  
5. (Lab-only) Click the author name in the page or paste the copied URL into the address bar — you should see an alert, proving JS execution in the vulnerable origin.

---

## Burp Suite workflow (Proxy + Repeater) — exact steps

**Prerequisite:** Configure browser to use Burp Proxy and enable Intercept.

### 1) Confirm reflection (benign marker)
- In browser: submit a comment with a benign marker in the Website field, e.g. `markerABC123`.  
- Burp intercepts the POST request → **Right-click → Send to Repeater**.  
- In Repeater: click **Go** to forward the request (store the comment).  
- In browser: load the post page (GET). Burp intercepts the GET response → **Send to Repeater**.  
- In Repeater response tab: search for `markerABC123`. Confirm presence inside an `href` attribute (example view in response body):
```html
<a href="markerABC123">AuthorName</a>
```
_Note:_ The response HTML source may show `href=&quot;markerABC123&quot;` (quote encoding). This is expected.

### 2) Submit the payload (edit POST in Repeater)
**Option A — Re-submit POST with payload (recommended & realistic):**
- In the Repeater tab containing the original POST, edit the Website parameter value to:
```text
javascript:alert(1)
```
- Click **Go** to submit the modified POST (this stores the payload if the server accepts it).  
- In browser: load the post page (GET). Intercept the GET response with Proxy → **Send to Repeater**.  
- Inspect the Repeater response body for the comment author anchor.

**Option B — (lab-only) Local proof:**  
- Fetch the GET response, save locally, modify the `href` to the payload and open locally. (This is only for offline verification and not representative of real stored exploitation.)

### 3) Verify injection in the response
- In Repeater response body, confirm the author anchor includes the payload:
```html
<a href="javascript:alert(1)">attacker</a>
```
- Or visible in source as encoded quotes:
```text
href=&quot;javascript:alert(1)&quot;
```
- Save these request/response snippets from Burp for evidence.

### 4) Confirm execution (lab)
- In a controlled environment, click the author link or paste the copied link into the browser address bar.  
- Expected: a JavaScript alert dialog appears (`alert(1)`), demonstrating XSS.

---


---
```
