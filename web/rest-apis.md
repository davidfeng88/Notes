# REST APIs

## Methods \(Verbs\)

* GET/POST/DELETE
* PUT: to replace a resource with another one
* PATCH: to modify a resource
* But sometimes the API designer may choose to let PUT, PATCH, and POST have the same behavior
* OPTIONS: returns all methods available in the end point
* HEAD: returns just the head of the response

## Request

Headers include Authorization, Content-Type \(e.g. `application/json`\), Cache-Control \(e.g. no-cache\), etc.

## Response

| HTTP response status code | Categories | Example |
| :--- | :--- | :--- |
| 1xx | Information | rarely used |
| 2xx | Success | 200 OK |
| 3xx | Redirection | 301 Moved permanently |
| 4xx | Client error | 404 Not found |
| 5xx | Server error | 503 Service unavailable |

