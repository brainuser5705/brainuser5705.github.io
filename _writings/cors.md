---
base-title: What is CORS?
base-description: what does it mean
time: May 27, 2025

draft: true
---

## What is CORS?

- Stands for *Cross Origin Resource Sharing*
- It is a safety mechanism from the browser to ensure that the outgoing requests are from the same domain. If the frontend is on `http://localhost:4200`, then the API that it requests from should also be on `http://localhost:4200`.
- Getting a CORS error doesn't necessarily mean that your API server is failing. The server could have very well received the request and successfully sent a response, but the browser will block it from entering back into the frontend.

## What are the sequence of checks performed by CORS?
1. A request to the API server is made from the frontend.
2. The browser will send a *preflight* `OPTIONS` request to check if the origin (of the API) is allowed.
3. The API server will read the request `Origin` header to see if it allows a request from the frontend.
4. The API server approves and must respond back to the frontend with the proper headers:
    ```
    Access-Control-Allow-Origin: <frontend-url>
    Access-Control-Allow-Origin: *
    ```

## Exceptions

If the request contains any custom headers (e.g. `Authorization`, `Content-Type: application/json`), then the browser send the preflight `OPTIONS`.

The `Access-Control-Allow-Origin` must return exact frontend origin. There must also be `Access-Control-Allow-Headers: <list of headers>`.

## Solutions for CORS
1. Avoid direct API requests made in the frontend (browser code). Migrate the calls to a backend server.
2. Add a proxy to your frontend. It will wrap the external API call with a call made to the local domain.
3. Configure the API gateway to inject the CORS response header (include the `Allow-Origin`) before returning to browser.