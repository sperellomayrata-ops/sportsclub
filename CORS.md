# CORS headers

**CORS** stands for **Cross-Origin Resource Sharing**. To understand CORS, we first need to understand the browser rule it overrides: the **Same-Origin Policy (SOP)**.

* **The rule (SOP):** By default, web browsers forbid a web page from making an AJAX request (using `fetch` or `axios`) to a different domain than the one the page came from. This is a security feature to prevent malicious sites from reading your private data on other sites.
* **The exception (CORS):** CORS is a mechanism that allows a server to tell the browser, "It's okay! I explicitly allow *this specific website* to request data from me."

The server communicates this permission by sending specific **HTTP headers** in its response. The most common one is:

```
Access-Control-Allow-Origin: https://www.example.com
```

## About `django-cors-headers`

Django is secure by default. It does not automatically send these CORS headers because it assumes you want to keep your data private to your own domain. However, modern web architecture often separates the frontend and backend. As an example, imagine this scenario:

* **Frontend:** React, Vue, or Angular app running on `http://localhost:3000`.
* **Backend:** Django or FastAPI REST API running on `http://localhost:8000`.

The browser sees `localhost:3000` and `localhost:8000` as **different origins** (different ports count as different origins). Without CORS headers, the browser would block our React app from talking to your Django API.

The `django-cors-headers` package is middleware that automatically adds the required headers (like `Access-Control-Allow-Origin`) to our Django responses so the browser accepts them.

## The "Blocked" Scenario

Imagine we are building a **student grade portal**.

* **Our frontend:** A React app hosted at `http://student-portal.com`.
* **Our backend:** A Django REST API hosted at `http://api.school-system.com`.

### Scenario A: Without `django-cors-headers`

1. The student logs into the React app.
2. The React app tries to `fetch('http://api.school-system.com/grades/')`.
3. The Browser sends the request to Django.
4. Django receives it and sends back the grades.
5. The **browser intercepts the response** and it looks for the CORS header.
6. **Missing Header:** Since Django did not send `Access-Control-Allow-Origin`, the browser **discards the data** and throws a red console error:
   ```
   Access to fetch at '...' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
   ```

### Scenario B: With `django-cors-headers`

1. You install and configure the package in Django.
2. The React app requests the grades.
3. Django processes the request and the `cors-headers` middleware adds:
   ```
   Access-Control-Allow-Origin: http://student-portal.com
   ```
4. The browser sees the header, matches the domain, and allows the React app to read the JSON data.

## What does it protect against?

It is important to clarify a common misconception: **CORS does not protect the backend, but it protects the user.**

If you did *not* have the `Same-Origin Policy`, and therefore did not need CORS:

1. You log into your bank website (`bank.com`). Your browser stores a session cookie.
2. You accidentally visit a malicious site (`evil-hacker.com`).
3. The site `evil-hacker.com` has a script that makes a request to `bank.com/transfer-money`.
4. Without `Same-Origin Policy`, your browser would send that request *with your bank session cookies*. The bank would process the transfer, thinking you authorized it.

**`Same-Origin Policy` blocks this:** The site `evil-hacker.com` cannot read the response from `bank.com` because the origins do not match. **CORS allows you to selectively lower this shield** only for domains you trust, like your own React frontend, while keeping `evil-hacker.com` blocked.
