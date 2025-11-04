# Understanding HTTP for Backend Engineers

**Video Source:** [YouTube - Understanding HTTP for Backend Engineers](https://youtu.be/a3C1DMswClQ?list=PLui3EUkuMTPgZcV0QhQrOcwMPcBCcd_Q1)

---

## 1. Introduction to HTTP

### What is HTTP?
- **Definition:** HTTP is the fundamental protocol for communication between web **clients** (browsers/apps) and **servers** [[00:11](http://www.youtube.com/watch?v=a3C1DMswClQ&t=11)]
- Facilitates the transfer of hypertext documents between clients and servers
- Foundation of data communication on the World Wide Web

### Two Core Ideas

#### Statelessness [[00:36](http://www.youtube.com/watch?v=a3C1DMswClQ&t=36)]
- HTTP has **no memory of past interactions**
- Each request is **self-contained** and must carry all necessary information [[01:05](http://www.youtube.com/watch?v=a3C1DMswClQ&t=65)]
  - Must include authentication tokens
  - Must include all necessary headers
  - Server doesn't maintain information about the client between requests
- **Benefits:**
  - Simplifies server architecture
  - Improves **scalability** [[01:35](http://www.youtube.com/watch?v=a3C1DMswClQ&t=95)]
  - Enables easier load balancing

#### Client-Server Model [[02:32](http://www.youtube.com/watch?v=a3C1DMswClQ&t=152)]
- Communication is **always initiated by the client** sending a request
- The server processes it and sends back an appropriate response [[03:25](http://www.youtube.com/watch?v=a3C1DMswClQ&t=205)]
- Clear separation of concerns

### HTTP vs HTTPS
- **HTTP:** Standard protocol
- **HTTPS [[03:33](http://www.youtube.com/watch?v=a3C1DMswClQ&t=213)]:** Secure version of HTTP
  - Utilizes **TLS** (Transport Layer Security)
  - Encryption and security certificates
  - Protects data in transit

### Connection Layer
- HTTP relies on **TCP** (Transmission Control Protocol) for establishing a reliable, connection-based communication link [[04:14](http://www.youtube.com/watch?v=a3C1DMswClQ&t=254)]

---

## 2. HTTP Versions & Connections

Evolution of HTTP protocol and performance improvements over time.

| Version | Key Features | Performance Impact |
|---------|-------------|-------------------|
| **HTTP 1.0** | Each request required a separate connection setup and closure [[05:55](http://www.youtube.com/watch?v=a3C1DMswClQ&t=355)] | Inefficient and slow due to connection overhead [[06:03](http://www.youtube.com/watch?v=a3C1DMswClQ&t=363)] |
| **HTTP 1.1** | Introduced **persistent connections** (keep-alive) to allow multiple requests/responses over a single connection [[06:11](http://www.youtube.com/watch?v=a3C1DMswClQ&t=371)] | Significantly improved performance [[06:20](http://www.youtube.com/watch?v=a3C1DMswClQ&t=380)] |
| **HTTP 2.0** | Added **multiplexing** (multiple requests over a single connection), **binary framing**, and **header compression** [[06:30](http://www.youtube.com/watch?v=a3C1DMswClQ&t=390)] | Improved speed but still faced Head-of-Line blocking [[07:07](http://www.youtube.com/watch?v=a3C1DMswClQ&t=427)] |
| **HTTP 3.0** | Built on the **QUIC** protocol over **UDP**, offering faster connection establishment and better handling of packet loss [[06:49](http://www.youtube.com/watch?v=a3C1DMswClQ&t=409)] | Enhanced speed and reliability by avoiding TCP's limitations [[06:54](http://www.youtube.com/watch?v=a3C1DMswClQ&t=414)] |

### Key Improvements Across Versions

#### HTTP/1.1 Limitations
- New TCP connection needed for each request
- Head-of-Line Blocking: Requests must complete in order
- No header compression
- Inefficiency: Multiple connections needed for parallel requests

#### HTTP/2.0 Improvements
- **Multiplexing:** Multiple requests/responses over single TCP connection
- **Header Compression:** HPACK compression reduces overhead
- **Binary Protocol:** More efficient parsing than text-based HTTP/1.1
- **Server Push:** Server can send resources before client requests
- Still faced some head-of-line blocking at TCP level

#### HTTP/3.0 Improvements
- **QUIC Protocol:** Built on UDP instead of TCP
- Faster connection establishment
- Better handling of packet loss
- Eliminates TCP head-of-line blocking
- Improved performance for mobile networks

---

## 3. HTTP Methods (Intent) [[01:06:19](http://www.youtube.com/watch?v=a3C1DMswClQ&t=3979)]

Methods define the intent of the client's action on the server's resource.

| Method | Action/Intent | Idempotent? | Description |
|--------|--------------|-------------|-------------|
| **GET** [[01:06:55](http://www.youtube.com/watch?v=a3C1DMswClQ&t=4015)] | Fetch data | Yes [[01:08:40](http://www.youtube.com/watch?v=a3C1DMswClQ&t=4120)] | Retrieves data without side effects, safe method |
| **POST** [[01:07:02](http://www.youtube.com/watch?v=a3C1DMswClQ&t=4022)] | Create new data | No [[01:09:27](http://www.youtube.com/watch?v=a3C1DMswClQ&t=4167)] | Creates a new resource each time, results in state change |
| **PUT** [[01:07:29](http://www.youtube.com/watch?v=a3C1DMswClQ&t=4049)] | Completely replace a resource | Yes [[01:08:53](http://www.youtube.com/watch?v=a3C1DMswClQ&t=4133)] | Updates entire resource or creates if doesn't exist |
| **PATCH** [[01:07:15](http://www.youtube.com/watch?v=a3C1DMswClQ&t=4035)] | Selectively update a resource | Yes (usually) [[01:08:13](http://www.youtube.com/watch?v=a3C1DMswClQ&t=4093)] | Partial modifications, append or selective change |
| **DELETE** [[01:07:57](http://www.youtube.com/watch?v=a3C1DMswClQ&t=4077)] | Remove a resource | Yes [[01:09:06](http://www.youtube.com/watch?v=a3C1DMswClQ&t=4146)] | Resource deleted after first successful call |
| **OPTIONS** [[01:10:11](http://www.youtube.com/watch?v=a3C1DMswClQ&t=4211)] | Query server capabilities | Yes | Used for CORS pre-flight requests |

### Method Details

#### GET
- **Purpose:** Fetch data from the server
- **Use Cases:** User profile, product listings, articles
- Safe and cacheable

#### POST
- **Purpose:** Create new resources
- **Use Cases:** User registration, form submissions, file uploads
- Not safe for retries without confirmation

#### PUT vs PATCH
- **PUT:** Replace entire resource (must send all fields)
- **PATCH:** Update only specific fields (more efficient for partial updates)

#### OPTIONS
- Special method for CORS pre-flight requests
- Queries server about supported methods and headers
- Critical for cross-origin security

---

## 4. HTTP Status Codes [[38:55](http://www.youtube.com/watch?v=a3C1DMswClQ&t=2335)]

Three-digit numbers that standardize communication on the result of a request.

| Series | Category | Common Codes | Description |
|--------|----------|--------------|-------------|
| **2xx** | **Success** [[42:20](http://www.youtube.com/watch?v=a3C1DMswClQ&t=2540)] | `200 OK`, `201 Created`, `204 No Content` | Request fulfilled successfully |
| **3xx** | **Redirection** [[43:34](http://www.youtube.com/watch?v=a3C1DMswClQ&t=2614)] | `301 Moved Permanently`, `304 Not Modified` | Further action required (e.g., use new URL or cache) |
| **4xx** | **Client Error** [[45:51](http://www.youtube.com/watch?v=a3C1DMswClQ&t=2751)] | `400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found`, `409 Conflict` | Error due to client (bad data, missing auth, no permission) |
| **5xx** | **Server Error** [[49:48](http://www.youtube.com/watch?v=a3C1DMswClQ&t=2988)] | `500 Internal Server Error`, `503 Service Unavailable` | Unexpected condition or failure on server side |

### 1xx - Informational
- Request received and process is continuing
- **100 Continue:** Client should continue with request
- Rarely used in practice

### 2xx - Success [[42:20](http://www.youtube.com/watch?v=a3C1DMswClQ&t=2540)]
Request successfully processed

- **200 OK:** Standard successful response
- **201 Created:** Resource successfully created (POST requests)
- **202 Accepted:** Request accepted but processing not complete (async operations)
- **204 No Content:** Successful request with no response body (DELETE operations)

### 3xx - Redirection [[43:34](http://www.youtube.com/watch?v=a3C1DMswClQ&t=2614)]
Further action needed to complete request

- **301 Moved Permanently:** Resource permanently moved to new URL
  - Search engines update their indexes
  - Browser caches the redirect
- **302 Found:** Temporary redirect
- **304 Not Modified:** Resource hasn't changed, use cached version
  - Critical for caching optimization

### 4xx - Client Errors [[45:51](http://www.youtube.com/watch?v=a3C1DMswClQ&t=2751)]
Request contains errors or cannot be fulfilled

- **400 Bad Request:** Malformed request syntax or invalid data
- **401 Unauthorized:** Authentication required or failed
- **403 Forbidden:** Server understands but refuses to authorize
- **404 Not Found:** Resource doesn't exist
- **405 Method Not Allowed:** HTTP method not supported for resource
- **409 Conflict:** Request conflicts with current state (e.g., duplicate resource)
- **429 Too Many Requests:** Rate limiting triggered

### 5xx - Server Errors [[49:48](http://www.youtube.com/watch?v=a3C1DMswClQ&t=2988)]
Server failed to fulfill valid request

- **500 Internal Server Error:** Generic server error, unexpected condition
- **502 Bad Gateway:** Invalid response from upstream server
- **503 Service Unavailable:** Server temporarily unable to handle request (maintenance, overload)
- **504 Gateway Timeout:** Upstream server timeout

---

## 5. HTTP Headers (Metadata) [[09:09](http://www.youtube.com/watch?v=a3C1DMswClQ&t=549)]

Key-value pairs carrying essential metadata about the request or response. Headers enable **extensibility** and **remote control** over the interaction [[01:14:15](http://www.youtube.com/watch?v=a3C1DMswClQ&t=4455)].

| Header Category | Purpose | Examples |
|----------------|---------|----------|
| **Request Headers** | Client environment/preferences [[11:24](http://www.youtube.com/watch?v=a3C1DMswClQ&t=684)] | `User-Agent`, `Authorization`, `Accept` |
| **Representation** | Details about the message body (payload) [[12:31](http://www.youtube.com/watch?v=a3C1DMswClQ&t=751)] | `Content-Type`, `Content-Length`, `Content-Encoding` |
| **Security** | Enhancing protection and policy enforcement [[13:15](http://www.youtube.com/watch?v=a3C1DMswClQ&t=795)] | `Strict-Transport-Security` (HSTS), `Content-Security-Policy` (CSP) |

### Request Headers [[11:24](http://www.youtube.com/watch?v=a3C1DMswClQ&t=684)]

Provide information about the client environment and preferences.

#### Host
- Specifies the domain name of the server
- Required in HTTP/1.1
- Example: `Host: api.example.com`

#### User-Agent
- Information about the client making the request
- Browser/client type and version
- Example: `User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)`

#### Accept
- Media types client can process
- Example: `Accept: application/json, text/html`
- Used for content negotiation

#### Accept-Language
- Client's preferred language [[01:03:34](http://www.youtube.com/watch?v=a3C1DMswClQ&t=3814)]
- Example: `Accept-Language: en-US, en;q=0.9`

#### Accept-Encoding
- Compression formats client supports [[01:07:37](http://www.youtube.com/watch?v=a3C1DMswClQ&t=4057)]
- Example: `Accept-Encoding: gzip, deflate, br`
- **Critical for reducing response sizes and saving bandwidth**

#### Authorization
- Credentials for authentication
- Example: `Authorization: Bearer <token>`

#### Content-Type
- Media type of request body
- Example: `Content-Type: application/json`

### Response Headers

Provide information about the response and how to handle it.

#### Content-Type [[12:31](http://www.youtube.com/watch?v=a3C1DMswClQ&t=751)]
- Indicates the media type of the returned resource
- Example: `Content-Type: application/json; charset=utf-8`

#### Content-Length
- Size of response body in bytes
- Example: `Content-Length: 1024`

#### Content-Encoding
- Compression algorithm used
- Example: `Content-Encoding: gzip`

#### Set-Cookie
- Sends cookies from server to client
- Used for session management
- Example: `Set-Cookie: sessionId=abc123; HttpOnly; Secure`

#### Cache-Control
- Directives for caching mechanisms
- Example: `Cache-Control: no-cache, no-store, must-revalidate`
- Example: `Cache-Control: max-age=3600` (cache for 1 hour)

#### ETag
- Unique identifier/hash of the resource
- Used for cache validation
- Example: `ETag: "33a64df551425fcc55e4d42a148795d9f25f89d4"`

#### Last-Modified
- Timestamp of when resource was last modified
- Example: `Last-Modified: Wed, 21 Oct 2024 07:28:00 GMT`

### Security Headers [[13:15](http://www.youtube.com/watch?v=a3C1DMswClQ&t=795)]

Critical for enhancing protection and policy enforcement.

#### Strict-Transport-Security (HSTS)
- Forces browsers to use HTTPS
- Example: `Strict-Transport-Security: max-age=31536000; includeSubDomains`

#### Content-Security-Policy (CSP)
- Controls which resources browser can load
- Prevents XSS attacks
- Example: `Content-Security-Policy: default-src 'self'; script-src 'self' 'unsafe-inline'`

#### Access-Control-Allow-Origin (CORS)
- Specifies allowed origins for cross-origin requests
- Example: `Access-Control-Allow-Origin: https://example.com`
- Example: `Access-Control-Allow-Origin: *` (allows all origins)

#### Access-Control-Allow-Methods
- Specifies allowed HTTP methods for CORS
- Example: `Access-Control-Allow-Methods: GET, POST, PUT, DELETE`

#### Access-Control-Allow-Headers
- Specifies allowed custom headers for CORS
- Example: `Access-Control-Allow-Headers: Content-Type, Authorization`

---

## 6. Cross-Origin Resource Sharing (CORS) [[20:52](http://www.youtube.com/watch?v=a3C1DMswClQ&t=1252)]

CORS is a **browser-enforced security mechanism** to control how web applications interact with resources hosted on a different domain (**cross-origin**).

### Why CORS Exists
- Prevents malicious websites from accessing sensitive data from other domains
- Same-origin policy would block all cross-origin requests by default
- CORS provides a controlled way to allow specific cross-origin requests

### Simple Request Flow [[21:44](http://www.youtube.com/watch?v=a3C1DMswClQ&t=1304)]

Used for basic requests that meet these conditions:
- Methods: `GET`, `POST`, or `HEAD`
- Simple headers only (Accept, Accept-Language, Content-Language, Content-Type)
- Content-Type limited to: `application/x-www-form-urlencoded`, `multipart/form-data`, or `text/plain`

**Flow:**
1. Browser sends request with `Origin` header
2. Server responds with `Access-Control-Allow-Origin` header [[22:21](http://www.youtube.com/watch?v=a3C1DMswClQ&t=1341)]
3. If origin matches, browser allows response to be read
4. If not, browser blocks the response

**Example:**
```
Request:
GET /api/users HTTP/1.1
Host: api.example.com
Origin: https://app.example.com

Response:
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://app.example.com
Content-Type: application/json
```

### Pre-flight Request Flow [[24:14](http://www.youtube.com/watch?v=a3C1DMswClQ&t=1454)]

Used for **complex requests** that don't meet simple request criteria:
- Methods: `PUT`, `PATCH`, `DELETE`
- Custom headers like `Authorization`
- `Content-Type: application/json`

**Flow:**
1. Browser sends preliminary **`OPTIONS`** request (pre-flight) [[26:25](http://www.youtube.com/watch?v=a3C1DMswClQ&t=1585)]
2. Pre-flight inquires about server's capabilities [[26:49](http://www.youtube.com/watch?v=a3C1DMswClQ&t=1609)]
   - What methods are allowed?
   - What headers are allowed?
   - What origins are allowed?
3. Server responds with CORS headers
4. If approved, browser sends the actual request [[29:25](http://www.youtube.com/watch?v=a3C1DMswClQ&t=1765)]
5. If not approved, browser blocks the actual request

**Example:**
```
Pre-flight Request:
OPTIONS /api/users HTTP/1.1
Host: api.example.com
Origin: https://app.example.com
Access-Control-Request-Method: DELETE
Access-Control-Request-Headers: Authorization

Pre-flight Response:
HTTP/1.1 204 No Content
Access-Control-Allow-Origin: https://app.example.com
Access-Control-Allow-Methods: GET, POST, PUT, DELETE
Access-Control-Allow-Headers: Authorization, Content-Type
Access-Control-Max-Age: 86400

Actual Request (if approved):
DELETE /api/users/123 HTTP/1.1
Host: api.example.com
Origin: https://app.example.com
Authorization: Bearer token123
```

### Key CORS Headers

| Header | Usage | Example |
|--------|-------|---------|
| `Access-Control-Allow-Origin` | Specifies allowed origins | `https://example.com` or `*` |
| `Access-Control-Allow-Methods` | Allowed HTTP methods | `GET, POST, PUT, DELETE` |
| `Access-Control-Allow-Headers` | Allowed request headers | `Content-Type, Authorization` |
| `Access-Control-Max-Age` | Pre-flight cache duration | `86400` (24 hours) |
| `Access-Control-Allow-Credentials` | Allow cookies/auth | `true` |

### Implementation Tips
- Never use `*` with credentials (cookies/auth)
- Cache pre-flight responses with `Max-Age`
- Be specific with allowed origins in production
- Handle OPTIONS method in your API

---

## 7. HTTP Caching [[55:24](http://www.youtube.com/watch?v=a3C1DMswClQ&t=3324)]

Storing copies of responses to reduce the need for repeated server requests, improving performance and reducing bandwidth.

### Cache Headers

#### Cache-Control [[56:47](http://www.youtube.com/watch?v=a3C1DMswClQ&t=3407)]
- Sets caching directives and max-age
- **Directives:**
  - `max-age=3600`: Cache for 3600 seconds
  - `no-cache`: Revalidate with server before using cache
  - `no-store`: Don't cache at all (sensitive data)
  - `public`: Can be cached by any cache (CDN, proxy)
  - `private`: Only browser cache, not CDN
  - `must-revalidate`: Must check with server when stale

#### ETag [[57:05](http://www.youtube.com/watch?v=a3C1DMswClQ&t=3425)]
- Unique hash/identifier of the resource
- Changes when resource content changes
- Example: `ETag: "33a64df551425fcc55e4d42a148795d9f25f89d4"`

#### Last-Modified [[57:24](http://www.youtube.com/watch?v=a3C1DMswClQ&t=3444)]
- Timestamp of when resource was last modified
- Example: `Last-Modified: Wed, 21 Oct 2024 07:28:00 GMT`

### Conditional GET (Cache Validation) [[59:06](http://www.youtube.com/watch?v=a3C1DMswClQ&t=3546)]

Efficient way to check if cached content is still valid.

**Flow:**
1. Client receives resource with `ETag` or `Last-Modified`
2. Client caches the resource
3. On subsequent request, client sends:
   - `If-None-Match: <etag>` (for ETag validation)
   - `If-Modified-Since: <date>` (for Last-Modified validation)
4. Server checks if resource changed:
   - **Not modified:** Returns `304 Not Modified` (no body, use cache)
   - **Modified:** Returns `200 OK` with new resource

**Example:**
```
Initial Request:
GET /api/users/123 HTTP/1.1

Initial Response:
HTTP/1.1 200 OK
ETag: "abc123"
Last-Modified: Mon, 01 Nov 2024 10:00:00 GMT
Cache-Control: max-age=3600
{ "id": 123, "name": "John" }

Subsequent Request (after cache expires):
GET /api/users/123 HTTP/1.1
If-None-Match: "abc123"
If-Modified-Since: Mon, 01 Nov 2024 10:00:00 GMT

Response (if not modified):
HTTP/1.1 304 Not Modified
ETag: "abc123"
(no body - use cached version)
```

### Caching Strategy
- Use `Cache-Control` for static assets (images, CSS, JS)
- Use `ETag` or `Last-Modified` for API responses
- Set appropriate `max-age` based on content freshness needs
- Use `private` for user-specific data
- Use `no-store` for sensitive data (passwords, payment info)

---

## 8. Content Negotiation [[01:02:36](http://www.youtube.com/watch?v=a3C1DMswClQ&t=3756)]

Mechanism where the client specifies its preferences (data format, language, encoding), and the server attempts to respond with a compatible format.

### Format/Media Type Negotiation [[01:03:26](http://www.youtube.com/watch?v=a3C1DMswClQ&t=3806)]

**Client specifies using `Accept` header:**
- `Accept: application/json` - Prefers JSON
- `Accept: application/xml` - Prefers XML
- `Accept: application/json, text/xml; q=0.9` - Prefers JSON, XML as fallback (quality value)

**Server responds with:**
- `Content-Type: application/json` - Indicates actual format sent

### Language Negotiation [[01:03:34](http://www.youtube.com/watch?v=a3C1DMswClQ&t=3814)]

**Client specifies using `Accept-Language` header:**
- `Accept-Language: en-US, en;q=0.9, fr;q=0.8`
- Quality values (q) indicate preference (0-1)

**Server responds with:**
- `Content-Language: en-US` - Indicates language used

### Compression/Encoding Negotiation [[01:07:37](http://www.youtube.com/watch?v=a3C1DMswClQ&t=4057)]

**Client specifies using `Accept-Encoding` header:**
- `Accept-Encoding: gzip, deflate, br`
- **Critical for reducing file sizes and saving bandwidth**

**Server responds with:**
- `Content-Encoding: gzip` - Indicates compression used
- Response body is compressed

**Benefits:**
- Can reduce response size by 70-90%
- Significantly improves load times
- Reduces bandwidth costs
- Modern compression algorithms (Brotli) even more efficient than gzip

### Quality Values (q)
- Range: 0.0 to 1.0 (default is 1.0)
- Higher value = higher preference
- Example: `Accept: text/html; q=1.0, application/json; q=0.8`

---

## 9. Handling Large Data

### Large Request - Upload [[01:11:07](http://www.youtube.com/watch?v=a3C1DMswClQ&t=4267)]

**For large file uploads:**
- Use `Content-Type: multipart/form-data` [[01:11:46](http://www.youtube.com/watch?v=a3C1DMswClQ&t=4306)]
- Transfers binary data in separate, delimited parts
- Each part can have its own headers

**Example:**
```
POST /api/upload HTTP/1.1
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="file"; filename="document.pdf"
Content-Type: application/pdf

[binary file data]
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="description"

Important document
------WebKitFormBoundary7MA4YWxkTrZu0gW--
```

**Best Practices:**
- Use chunked transfer encoding for very large files
- Implement progress tracking
- Support resume capability for interrupted uploads
- Validate file size and type on server

### Large Response - Download [[01:13:42](http://www.youtube.com/watch?v=a3C1DMswClQ&t=4422)]

**For large responses or streaming data:**
- Use `Content-Type: text/event-stream` for Server-Sent Events
- Use `Connection: keep-alive` header
- Stream data in **chunks**
- Client continuously receives and processes data

**Example (Server-Sent Events):**
```
GET /api/stream HTTP/1.1

HTTP/1.1 200 OK
Content-Type: text/event-stream
Connection: keep-alive
Cache-Control: no-cache

data: {"id": 1, "message": "First chunk"}

data: {"id": 2, "message": "Second chunk"}

data: {"id": 3, "message": "Third chunk"}
```

**Use Cases:**
- Real-time notifications
- Live updates (stock prices, sports scores)
- Log streaming
- AI model streaming responses
- Large file downloads

**Benefits:**
- Client can start processing before entire response received
- Reduces memory usage
- Better user experience (progressive rendering)
- Handles network interruptions better

---

## 10. RESTful APIs

REST (Representational State Transfer) principles for building scalable APIs.

### Core Principles

#### 1. Statelessness
- Each request contains all information needed
- No server-side session state
- Benefits: Scalability, simplicity, reliability

#### 2. Client-Server Architecture
- Separation of concerns
- Client handles UI/UX
- Server handles data/business logic
- Enables independent evolution

#### 3. Uniform Interface
- Standardized way to interact with resources
- Resources identified by URIs
- Self-descriptive messages
- HATEOAS (Hypermedia as Engine of Application State)

#### 4. Cacheability
- Responses must define if cacheable
- Improves performance and scalability

#### 5. Layered System
- Client doesn't know if connected to end server or intermediary
- Enables load balancers, proxies, gateways

### Resource Representation
- **URI Structure:** `/api/v1/users/{userId}`
- **Multiple Representations:** 
  - JSON (most common)
  - XML
  - HTML
- **Versioning:** Include version in URI or header

---

## 11. Best Practices for Backend Engineers

### Use Appropriate HTTP Methods
- GET for reading
- POST for creating
- PUT/PATCH for updating
- DELETE for removing
- Maintains semantic clarity
- Enables proper caching

### Implement Proper Status Codes
- Return accurate codes for each scenario
- Don't use 200 OK for errors
- Helps clients handle responses correctly
- Improves API usability

### Secure Endpoints
- **Always use HTTPS** in production
- Implement authentication (JWT, OAuth)
- Validate input data
- Sanitize output
- Use rate limiting
- Implement CORS properly

### Optimize Performance
- **Caching Headers:** Use `Cache-Control`, `ETag`, `Last-Modified`
- **HTTP/2 or HTTP/3:** Upgrade for better performance
- **Compression:** Enable gzip/brotli compression (70-90% size reduction)
- **CDN:** Use Content Delivery Networks for static assets
- **Connection Pooling:** Reuse connections
- **Pagination:** Limit large result sets
- **Streaming:** Use for large responses

### API Design
- **Consistent Naming:** Use kebab-case or snake_case consistently
- **Versioning:** Plan for API evolution (`/api/v1/`, `/api/v2/`)
- **Documentation:** Use OpenAPI/Swagger
- **Error Messages:** Provide clear, actionable error messages
- **Rate Limiting:** Protect against abuse (use 429 status code)
- **Idempotency:** Ensure PUT, PATCH, DELETE are idempotent
- **Resource Hierarchies:** Use logical URL structure

---

## Key Takeaways

1. **HTTP is fundamental** - Understanding it deeply is essential for backend engineering
2. **Statelessness enables scalability** - Each request is self-contained
3. **HTTP versions matter** - HTTP/2 and HTTP/3 provide significant performance improvements
4. **Choose correct methods** - Use GET, POST, PUT, PATCH, DELETE appropriately
5. **Status codes communicate clearly** - Use appropriate codes for each scenario
6. **Headers provide critical metadata** - For authentication, caching, content negotiation
7. **CORS security** - Understand simple vs pre-flight requests
8. **Caching is crucial** - Use ETag, Last-Modified, and Cache-Control effectively
9. **Content negotiation** - Support compression, multiple formats, languages
10. **Handle large data efficiently** - Use multipart/form-data for uploads, streaming for downloads
11. **Security first** - Always use HTTPS, implement proper CORS, security headers
12. **REST principles** - Follow statelessness, uniform interface, cacheability
13. **Optimization** - Compression can reduce size by 70-90%
14. **Idempotency matters** - Ensures safe retries for certain operations

---

## Related Topics to Explore

- **HTTP/3 and QUIC** - Next generation protocol with improved performance
- **WebSockets** - Full-duplex communication for real-time applications
- **GraphQL** - Alternative to REST with flexible queries
- **gRPC** - High-performance RPC framework
- **API Gateway patterns** - Centralized API management
- **Rate limiting algorithms** - Token bucket, leaky bucket
- **OAuth 2.0 and OpenID Connect** - Modern authentication/authorization
- **TLS/SSL certificate management** - Securing HTTPS connections
- **Load balancing strategies** - Round-robin, least connections, consistent hashing
- **CDN architecture** - Content delivery optimization
- **API versioning strategies** - URL vs header vs content negotiation
- **Microservices communication** - Service mesh, circuit breakers

---

## Summary Tables

### HTTP Methods Comparison

| Method | Purpose | Idempotent | Safe | Cacheable |
|--------|---------|-----------|------|-----------|
| GET | Read | ✓ | ✓ | ✓ |
| POST | Create | ✗ | ✗ | Rarely |
| PUT | Replace | ✓ | ✗ | ✗ |
| PATCH | Update | ✓* | ✗ | ✗ |
| DELETE | Remove | ✓ | ✗ | ✗ |
| OPTIONS | Query | ✓ | ✓ | ✗ |

*Usually idempotent, depends on implementation

### HTTP Version Evolution

| Feature | HTTP/1.0 | HTTP/1.1 | HTTP/2 | HTTP/3 |
|---------|---------|---------|--------|--------|
| Connection | New per request | Persistent | Persistent | Persistent |
| Multiplexing | ✗ | ✗ | ✓ | ✓ |
| Header Compression | ✗ | ✗ | ✓ (HPACK) | ✓ (QPACK) |
| Protocol | Text | Text | Binary | Binary |
| Transport | TCP | TCP | TCP | UDP (QUIC) |
| Head-of-Line Blocking | ✓ | ✓ | Partial | ✗ |

### Common Status Code Usage

| Code | Name | When to Use |
|------|------|-------------|
| 200 | OK | Successful GET, PUT, PATCH |
| 201 | Created | Successful POST |
| 204 | No Content | Successful DELETE |
| 304 | Not Modified | Cache validation passed |
| 400 | Bad Request | Invalid input data |
| 401 | Unauthorized | Missing/invalid authentication |
| 403 | Forbidden | No permission |
| 404 | Not Found | Resource doesn't exist |
| 409 | Conflict | Duplicate resource |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Unexpected server error |
| 503 | Service Unavailable | Server overloaded/maintenance |



