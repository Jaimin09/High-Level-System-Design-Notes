
# API Architecture and REST API Best Practices

## Key API Design Best Practices

### 1. Master HTTP Fundamentals
- Know HTTP verbs: `GET` (Read), `POST` (Create), `PUT`/`PATCH` (Update), `DELETE` (Delete).
- Understand how status codes work: 2xx (success), 4xx (client error), 5xx (server error).
- Resources are accessed via URIs: `/books/`, `/users/`.
- The verb *in the request* (not in the URI) tells you what action is performed (e.g., `POST /books/` to create a book).

### 2. Use JSON with Proper Headers
- Responses should be `application/json` (include correct `Content-Type` header).
- Do not return plain text

### 3. Donâ€™t Use Verbs in URIs
- URIs should represent resources, **not actions**.
    - Bad: `/books/createNewBook/`
    - Good: `/books/` (verb comes from HTTP method).

### 4. Use Plural Nouns for Resources
- Consistently use plural for resource names, e.g., `/books/`, `/users/` (to avoid ambiguity and follow conventions).

### 5. Error Handling
- Do not return error with 200 status code.
- Error responses should be in JSON, e.g.,
  ```json
  { "error": "Invalid payload.", "detail": { "name": "This field is required." } }
  ```
- Use descriptive and precise error messages.

### 6. Use HTTP Status Codes Consistently
- Donâ€™t always return `200 OK` for failures!
- Map CRUD and error outcomes to the right HTTP codes:
  - `GET`: 200 OK
  - `POST`: 201 Created
  - `PATCH/PUT`: 200 OK
  - `DELETE`: 204 No Content
- For errors, use 4xx for client mistakes, 5xx for server problems.

### 7. Flat Resource Structures > Nesting
- Prefer flat resource design over deeply nested URIs.
  - OK: `/books?author=Cagan`
  - Discouraged: `/authors/Cagan/books/` (sometimes still used if logical)
 
### 8. Query Strings for Filtering and Pagination
- Use query parameters for filtering and pagination:
  - Example: `/books?author=Cagan&page=2&page_size=10`

### 10. Know the Difference: 401 vs 403
- `401 Unauthorized`: Missing/invalid authentication credentials.
- `403 Forbidden`: Authenticated, but not allowed to access this resource.

### 11. Use 202 Accepted for Deferred Processing
- Use this status when the server accepts the request but processing is asynchronous or will be completed later.

## Additional MUST-KNOW HTTP and REST API Info

### Whatâ€™s the difference between HTTP APIs and REST APIs?
- **HTTP API**: Uses HTTP protocol; basic; possibly fewer constraints; fast for internal/simple usage.
- **REST API**: Subtype of HTTP API that strictly follows REST constraints (stateless, client/server split, resource-based, etc.). More scalable and flexible for integration, especially in microservices.

#### REST API Principles (from MDN and RESTful API Guides)
- **Stateless**: No client session is stored on the server; every request is independent.
- **Client-Server**: The client (frontend) and server (backend) are separate and interact via requests and responses.
- **Cacheable**: Cache responses as appropriate.
- **Uniform Interface**: Standardized, predictable, and consistent endpoints and error formats.
- **Layered System**: Different roles (proxy, gateway, etc.) can be composed as needed.

## Most Common HTTP Status Codes in REST APIs

### Client Errors (4xx)
| Code | Meaning                       | When to Use                                    |
|------|-------------------------------|------------------------------------------------|
| 400  | Bad Request                   | Malformed syntax, invalid input                |
| 401  | Unauthorized                  | No/invalid authentication sent                 |
| 403  | Forbidden                     | Authenticated, but user lacks permission       |
| 404  | Not Found                     | Resource doesnâ€™t exist                         |
| 405  | Method Not Allowed            | Method not allowed on this endpoint            |
| 408  | Request Timeout               | Client took too long to send request           |

### Server Errors (5xx)
| Code | Meaning                    | When to Use                                        |
|------|----------------------------|----------------------------------------------------|
| 500  | Internal Server Error      | Generic (server failed for unknown reason)         |
| 501  | Not Implemented            | Method not supported                              |
| 502  | Bad Gateway                | Proxy/gateway got invalid response from upstream   |
| 503  | Service Unavailable        | Server busy, under maintenance, or overloaded      |
| 504  | Gateway Timeout            | Proxy/gateway timed out waiting for upstream       |

