# API Protocol

- [SOAP (Simple Object Access Protocol)](#soap-simple-object-access-protocol)
    - [How SOAP Works?](#how-soap-works)
    - [Best-Suited Use Cases](#best-suited-use-cases)
    - [Pros of SOAP](#pros-of-soap)
    - [Cons of SOAP](#cons-of-soap)
    - [Real-Life Examples of SOAP](#real-life-examples-of-soap)
- [REST (Representational State Transfer)](#rest-representational-state-transfer)
    - [How REST Works?](#how-rest-works)
    - [Best-Suited Use Cases](#best-suited-use-cases)
    - [Pros of REST](#pros-of-rest)
    - [Cons of REST](#cons-of-rest)
    - [Real-Life Examples of REST](#real-life-examples-of-rest)
- [GraphQL](#graphql)
    - [How GraphQL Works?](#how-graphql-works)
    - [Best-Suited Use Cases](#best-suited-use-cases)
    - [Pros of GraphQL](#pros-of-graphql)
    - [Cons of GraphQL](#cons-of-graphql)
    - [Real-Life Examples of GraphQL](#real-life-examples-of-graphql)
- [gRPC (Google Remote Procedure Call)](#grpc-google-remote-procedure-call)
    - [How gRPC Works?](#how-grpc-works)
    - [Best-Suited Use Cases](#best-suited-use-cases)
    - [Pros of gRPC](#pros-of-grpc)
    - [Cons of gRPC](#cons-of-grpc)
    - [Real-Life Examples of gRPC](#real-life-examples-of-grpc)
- [WebSocket](#websocket)
    - [How WebSocket Works?](#how-websocket-works)
    - [Best-Suited Use Cases](#best-suited-use-cases)
    - [Pros of WebSocket](#pros-of-websocket)
    - [Cons of WebSocket](#cons-of-websocket)
    - [Real-Life Examples of WebSocket](#real-life-examples-of-websocket)
- [Webhook](#webhook)
    - [How Webhook Works?](#how-webhook-works)
    - [Best-Suited Use Cases](#best-suited-use-cases)
    - [Pros of Webhooks](#pros-of-webhooks)
    - [Cons of Webhooks](#cons-of-webhooks)
    - [Real-Life Examples of Webhooks](#real-life-examples-of-webhooks)
- [Comparison of API Protocols](#comparison-of-api-protocols)
    - [Key Takeaways](#key-takeaways)
- [Deep Dive into RESTful APIs](#deep-dive-into-restful-apis)
    - [Key Components of REST APIs - HTTP Methods (CRUD Operations)](#key-components-of-rest-apis---http-methods-crud-operations)
    - [REST API Request Structure](#rest-api-request-structure)
    - [Query Parameters (`?key=value`)](#query-parameters-keyvalue)
    - [Headers (`key: value`)](#headers-key-value)
    - [Request Body (Payload)](#request-body-payload)
    - [REST API Response Structure](#rest-api-response-structure)
    - [Authentication in REST APIs](#authentication-in-rest-apis)
    - [REST API Best Practices](#rest-api-best-practices)
    - [When NOT to Use REST APIs?](#when-not-to-use-rest-apis)
- [Pagination in REST APIs](#pagination-in-rest-apis)
    - [Why Pagination is Needed](#why-pagination-is-needed)
    - [Offset-Based Pagination](#offset-based-pagination)
    - [Cursor-Based Pagination](#cursor-based-pagination)
    - [Page-Based Pagination](#page-based-pagination)
    - [Pagination Parameters](#pagination-parameters)
    - [Best Practices for Pagination in REST APIs](#best-practices-for-pagination-in-rest-apis)
- [Deep Dive into GraphQL APIs](#deep-dive-into-graphql-apis)
    - [How Does GraphQL Work?](#how-does-graphql-work)
    - [Schema](#schema)
    - [Resolvers](#resolvers)
    - [GraphQL Query Structure](#graphql-query-structure)
    - [Client-Specified Queries (Key Feature of GraphQL)](#client-specified-queries-key-feature-of-graphql)
    - [Single Endpoint ****(Key Feature of GraphQL)](#single-endpoint-key-feature-of-graphql)
    - [Real-time Updates with Subscriptions (Key Feature of GraphQL)](#real-time-updates-with-subscriptions-key-feature-of-graphql)
    - [Advantages of GraphQL](#advantages-of-graphql)
    - [Disadvantages of GraphQL](#disadvantages-of-graphql)
    - [When to Use GraphQL vs REST](#when-to-use-graphql-vs-rest)
    - [Best Practices for GraphQL APIs](#best-practices-for-graphql-apis)

# SOAP (Simple Object Access Protocol)

[**Simple Object Access Protocol Pros and Cons (Explained by Example)**](https://youtu.be/it8ybkQuAh8)

SOAP (Simple Object Access Protocol) is a protocol used for exchanging structured information in web services. It is an XML-based messaging protocol that enables communication between applications running on different operating systems and programming languages over the internet. SOAP operates over various transport protocols such as HTTP, SMTP, TCP, and more.

### How SOAP Works?

SOAP messages are formatted as XML documents and typically include:

1. **Envelope:** The root element defining the message structure.
2. **Header:** Contains optional metadata, such as authentication or routing information.
3. **Body:** Holds the actual request/response data.
4. **Fault (Optional):** Used for error handling.

SOAP communicates using the **request-response** model. A client sends a SOAP request to the server, and the server responds with a SOAP response. Since SOAP uses XML, it is **platform-independent** and **language-neutral**.

SOAP APIs are defined using **WSDL (Web Services Description Language)**, which describes the available endpoints, methods, and data formats.

### Best-Suited Use Cases

1. **Enterprise Applications** – Banking, finance, insurance, and government systems that require strict security and compliance.
2. **Payment Gateways** – Secure transactions, such as PayPal’s API.
3. **Telecom Services** – Mobile service providers use SOAP for reliable message exchange.
4. **Healthcare Systems** – Compliance-heavy industries like healthcare, requiring strong security and ACID transactions.
5. **Legacy System Integration** – Systems that rely on older protocols like RPC or CORBA.

### Pros of SOAP

1. **Highly Secure** – Supports WS-Security for authentication, encryption, and digital signatures.
2. **Reliable Messaging** – Supports ACID-compliant transactions and error handling via the Fault element.
3. **Standardized** – Uses WSDL for formal contracts, making integration more structured.
4. **Language & Platform Independent** – Works with different OS and programming languages.
5. **Supports Multiple Transport Protocols** – Unlike REST, which mainly uses HTTP, SOAP can work over SMTP, TCP, FTP, etc.

### Cons of SOAP

1. **Heavy and Slow** – XML messages are large, making SOAP less efficient in performance compared to REST.
2. **Complexity** – Requires strict XML structure and WSDL contracts, making it harder to implement.
3. **Overhead** – More bandwidth and processing power needed due to extensive XML parsing.
4. **Limited Browser Support** – Since SOAP is not optimized for web applications, it does not work well with AJAX.

### Real-Life Examples of SOAP

1. **Banking & Financial Services:** PayPal APIs for secure money transfers.
2. **Enterprise Applications:** SAP and Oracle use SOAP for their web services.
3. **Telecommunications:** Verizon and AT&T use SOAP-based APIs for managing mobile networks.
4. **Healthcare Systems:** HL7 standard for electronic health records (EHR) relies on SOAP.
5. **Payment Processing:** Credit card networks like Visa use SOAP for secure transactions.

# REST (Representational State Transfer)

[**What Is REST API? Examples And How To Use It**](https://youtu.be/-mN3VyJuCjM)

[**The Right Way To Build REST APIs**](https://youtu.be/CVBpYfPKGlE)

[**What is an API and how do you design it?**](https://youtu.be/_YlYuNMTCc8)

[**Good APIs Vs Bad APIs: 7 Tips for API Design**](https://youtu.be/_gQaygjm_hg)

REST (Representational State Transfer) is an architectural style used for designing networked applications. It relies on stateless communication and a uniform interface, typically using **HTTP** as the underlying transport protocol. RESTful APIs are **lightweight, scalable, and flexible**, making them a preferred choice for web and mobile applications.

### How REST Works?

RESTful APIs use standard HTTP methods to perform operations on resources, which are identified by **URIs (Uniform Resource Identifiers)**. These operations include:

1. **GET** – Retrieve data from the server.
2. **POST** – Create a new resource on the server.
3. **PUT** – Update an existing resource.
4. **PATCH** – Partially update an existing resource.
5. **DELETE** – Remove a resource from the server.

REST follows the **stateless** principle, meaning each request must contain all necessary information, and the server does not store client state between requests. Data is typically exchanged in **JSON** or **XML** format, with JSON being more common due to its lightweight nature.

### Best-Suited Use Cases

1. **Web and Mobile Applications** – Ideal for front-end and back-end communication (e.g., social media, e-commerce).
2. **Microservices Architecture** – RESTful APIs help microservices communicate efficiently.
3. **Public APIs** – Widely used in third-party integrations (e.g., Twitter, Google Maps).
4. **IoT and Cloud-based Services** – Used in cloud platforms like AWS and Google Cloud.
5. **Streaming Services** – Netflix and Spotify use REST APIs for content delivery.

### Pros of REST

1. **Scalable and Fast** – Uses lightweight JSON over HTTP, reducing bandwidth and latency.
2. **Stateless** – Each request is independent, improving performance and scalability.
3. **Simple and Easy to Use** – Uses standard HTTP methods and is easy to understand.
4. **Widely Adopted** – REST is the default choice for most web applications and cloud services.
5. **Language and Platform Independent** – Works with any programming language supporting HTTP.

### Cons of REST

1. **No Built-in Security** – Relies on HTTPS, OAuth, or other mechanisms for security.
2. **Lack of Strict Standards** – Unlike SOAP, REST does not enforce a strict contract like WSDL.
3. **Over-fetching or Under-fetching** – Clients may receive more or less data than needed.
4. **Stateless Nature** – While beneficial for scalability, it can make complex transactions harder.
5. **Limited to HTTP** – Unlike SOAP, which can use multiple transport protocols.

### Real-Life Examples of REST

1. **Social Media APIs** – Facebook, Twitter, and Instagram provide RESTful APIs.
2. **E-commerce Platforms** – Amazon and Shopify use REST APIs for order management.
3. **Cloud Services** – AWS, Google Cloud, and Azure offer REST-based APIs.
4. **Streaming Services** – Netflix, Spotify, and YouTube rely on REST for data retrieval.
5. **Mapping Services** – Google Maps and OpenStreetMap provide RESTful APIs for location data.

# GraphQL

[**What Is GraphQL? REST vs. GraphQL**](https://youtu.be/yWzKJPw_VzM)

[**Learn GraphQL in 7 Minutes For Beginners**](https://youtu.be/Zg4XIpnLWQg)

GraphQL is a **query language** and runtime for APIs, developed by Facebook in 2012 and open-sourced in 2015. It allows clients to request exactly the data they need, reducing **over-fetching** and **under-fetching** problems common in REST APIs. Unlike REST, which has multiple endpoints for different resources, GraphQL exposes a **single endpoint** that can handle flexible queries.

### How GraphQL Works?

GraphQL APIs are built around three key components:

1. **Schema:** Defines the types of data and relationships in the API.
2. **Queries:** Used to fetch data from the API, similar to `GET` in REST.
3. **Mutations:** Used to modify data, similar to `POST`, `PUT`, or `DELETE` in REST.

A GraphQL request is structured as a single query, where clients specify **exactly** what fields they need. The server processes the request and returns only the requested fields, reducing data transfer overhead.

**Example Query:**

```graphql
query {
  user(id: "123") {
    name
    email
    friends {
      name
    }
  }
}
```

**Example Response:**

```json
{
  "data": {
    "user": {
      "name": "John Doe",
      "email": "john@example.com",
      "friends": [
        { "name": "Alice" },
        { "name": "Bob" }
      ]
    }
  }
}
```

This allows clients to avoid unnecessary data and get only what they need.

### Best-Suited Use Cases

GraphQL is best used in:

1. **Mobile and Web Applications** – Reduces unnecessary data transfer, improving performance.
2. **Microservices and Aggregated APIs** – Combines multiple APIs into a single GraphQL endpoint.
3. **Social Media Platforms** – Facebook, Twitter, and GitHub use GraphQL to handle complex user relationships.
4. **E-commerce Applications** – GraphQL helps fetch product, user, and order details in a single request.
5. **Real-time Applications** – With **GraphQL Subscriptions**, real-time updates are supported (e.g., chat apps, stock market updates).

### Pros of GraphQL

1. **Efficient Data Fetching** – Clients request only what they need, avoiding over-fetching or under-fetching.
2. **Single Endpoint** – Unlike REST, GraphQL has a single `/graphql` endpoint, simplifying API management.
3. **Strongly Typed Schema** – Ensures clear API structure and reduces errors.
4. **Great for Frontend Development** – Frontend teams can modify queries without backend changes.
5. **Supports Real-time Data** – GraphQL **subscriptions** enable live data updates.

### Cons of GraphQL

1. **Complexity** – Requires learning a new query language and managing resolvers.
2. **Caching Challenges** – Unlike REST, GraphQL’s dynamic queries make traditional HTTP caching difficult.
3. **Performance Overhead** – Query resolution can be slow for deeply nested queries.
4. **Security Concerns** – Poorly optimized queries can lead to **DoS (Denial of Service) attacks** if clients request excessive data.
5. **Overhead in Small Applications** – Simple REST APIs might be more efficient for basic CRUD operations.

### Real-Life Examples of GraphQL

1. **Facebook:** Originally developed GraphQL for their mobile apps.
2. **GitHub:** Uses GraphQL for its API, allowing developers to fetch user repos, issues, and more in a single request.
3. **Shopify:** Uses GraphQL to provide flexible product data retrieval for merchants.
4. **Netflix:** Uses GraphQL to manage complex media catalogs efficiently.
5. **Twitter:** Uses GraphQL to fetch user timelines and interactions.

# gRPC (Google Remote Procedure Call)

gRPC is a **high-performance, open-source RPC (Remote Procedure Call) framework** developed by Google. It enables communication between services using **protocol buffers (Protobuf)** as the data serialization format instead of JSON or XML. gRPC is designed for **low-latency, high-throughput applications**, making it a great choice for microservices, real-time systems, and inter-service communication.

### How gRPC Works?

gRPC follows a **client-server model**, where clients call remote functions as if they were local. It uses:

1. **Protocol Buffers (Protobuf):** A lightweight, efficient binary serialization format that defines API contracts.
2. **HTTP/2:** Enables multiplexing, reducing latency and improving performance.
3. **Streaming Support:** Allows real-time bidirectional data exchange.

**Steps to Use gRPC:**

1. Define the API in a `.proto` file using Protobuf.
2. Generate client and server code using gRPC tools.
3. Implement the server and call methods from the client.

**Example `.proto` Definition:**

```
syntax = "proto3";

service UserService {
  rpc GetUser (UserRequest) returns (UserResponse);
}

message UserRequest {
  string user_id = 1;
}

message UserResponse {
  string name = 1;
  string email = 2;
}
```

The gRPC framework auto-generates client and server stubs for multiple languages, reducing development effort.

### Best-Suited Use Cases

1. **Microservices Communication** – Fast and efficient inter-service communication.
2. **Real-time Streaming Applications** – Video streaming, gaming, and IoT applications.
3. **Cloud-Native Systems** – Kubernetes and cloud platforms use gRPC for service communication.
4. **Low-Latency Applications** – Financial trading systems, AI inference, and high-performance APIs.
5. **Multi-language Systems** – Supports multiple programming languages like Python, Java, Go, and C++.

### Pros of gRPC

1. **High Performance** – Uses Protobuf (binary format) and HTTP/2 for low-latency communication.
2. **Streaming Support** – Supports **unary, client, server, and bidirectional streaming**.
3. **Automatic Code Generation** – Reduces manual work by generating client and server code.
4. **Strongly Typed API** – Protobuf enforces structured communication, reducing errors.
5. **Multi-language Support** – Works across different programming languages efficiently.

### Cons of gRPC

1. **More Complex than REST** – Requires learning Protobuf and gRPC tooling.
2. **Limited Browser Support** – Browsers do not natively support HTTP/2-based gRPC.
3. **Harder Debugging** – Binary Protobuf format is not human-readable like JSON.
4. **Overhead for Simple APIs** – REST might be a better choice for basic CRUD operations.
5. **Caching Challenges** – Unlike REST, gRPC lacks built-in support for HTTP-level caching.

### Real-Life Examples of gRPC

1. **Google Cloud Services:** Google uses gRPC internally for cloud APIs.
2. **Netflix:** Uses gRPC for efficient microservices communication.
3. **Dropbox:** Uses gRPC to optimize performance in file-sharing services.
4. **Uber:** Uses gRPC for handling millions of ride requests efficiently.
5. **Spotify:** Uses gRPC to manage real-time music streaming and metadata retrieval.

# WebSocket

WebSocket is a **full-duplex communication protocol** that enables real-time, two-way interaction between a client (browser or app) and a server over a single, long-lived connection. Unlike REST or gRPC, which follow a request-response pattern, WebSocket maintains an open connection, allowing data to be pushed instantly from the server to the client and vice versa.

### How WebSocket Works?

WebSocket operates over **TCP** and follows a **handshake mechanism** over HTTP(S) to establish a persistent connection. The handshake looks like this:

1. The client sends a **WebSocket upgrade request** using HTTP headers.
2. The server responds with `101 Switching Protocols`, upgrading the connection.
3. Once established, messages can flow in both directions **without re-establishing the connection**.

**WebSocket Handshake Example:**

```
Client request:
GET /chat HTTP/1.1
Host: example.com
Connection: Upgrade
Upgrade: websocket
Sec-WebSocket-Key: random_base64==
Sec-WebSocket-Version: 13

Server response:
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: encoded_key
```

After this handshake, the connection remains **open** for real-time communication, avoiding the overhead of repeated HTTP requests.

### Best-Suited Use Cases

1. **Real-time Chat Applications** – WhatsApp, Messenger, Slack, and Discord use WebSockets for instant messaging.
2. **Live Notifications** – Stock market updates, sports scores, and social media alerts.
3. **Online Gaming** – Multiplayer games need low-latency communication.
4. **Live Streaming** – Video, audio, and event-based streaming platforms.
5. **Collaborative Applications** – Google Docs, Miro, and Figma use WebSockets for real-time collaboration.

### Pros of WebSocket

1. **Real-time, Bidirectional Communication** – Unlike REST, WebSocket allows instant message exchange between client and server.
2. **Low Latency & Efficient** – Uses a persistent connection, reducing network overhead.
3. **Less Bandwidth Usage** – No need for repeated HTTP requests, unlike REST polling.
4. **Supports Streaming** – Ideal for live updates and media streaming.
5. **Widely Supported** – Supported in all modern browsers and frameworks.

### Cons of WebSocket

1. **More Complex than REST** – Requires managing persistent connections.
2. **Not Cacheable** – Unlike REST APIs, WebSocket messages cannot be cached.
3. **Firewall & Proxy Issues** – Some firewalls and proxies may block WebSocket connections.
4. **Scalability Challenges** – Maintaining thousands of open connections requires optimized infrastructure.
5. **Less Secure by Default** – Must be used with **WSS (WebSocket Secure)** for encryption.

### Real-Life Examples of WebSocket

1. **WhatsApp, Messenger, Slack** – Instant messaging apps use WebSockets for real-time chat.
2. **Stock Market Apps** – Trading platforms like Robinhood use WebSockets for live price updates.
3. **Online Multiplayer Games** – Games like Fortnite and PUBG use WebSockets for real-time player interactions.
4. **Google Docs & Figma** – Real-time collaboration tools use WebSockets for instant updates.
5. **Sports Score Apps** – ESPN and other platforms push live score updates using WebSockets.

# Webhook

A **Webhook** is a mechanism that allows real-time communication between applications using **event-driven HTTP callbacks**. Unlike REST APIs, where the client repeatedly polls for new data, Webhooks **push data automatically** when an event occurs, reducing unnecessary requests.

Webhooks are commonly used for **notifications, automation, and integrations** between different services.

### How Webhook Works?

1. **Client Registers a Webhook URL** – The receiver (client) provides an endpoint (URL) where it wants to receive notifications.
2. **Event Occurs on the Server** – A predefined event (e.g., a new user sign-up) triggers the webhook.
3. **Server Sends a POST Request** – The server makes an HTTP `POST` request to the client’s webhook URL with relevant data.
4. **Client Processes the Data** – The client receives the webhook data and takes appropriate action.

**Example:** GitHub Webhook for Push Events

- You configure a webhook in a GitHub repository.
- When someone pushes code, GitHub sends a `POST` request to your webhook URL with the commit details.
- Your server receives and processes the data (e.g., triggering a CI/CD pipeline).

**Example Webhook Payload (JSON):**

```json
{
  "event": "push",
  "repository": "example-repo",
  "pusher": "john_doe",
  "commit_message": "Fixed a bug in authentication"
}
```

### Best-Suited Use Cases

1. **Real-Time Notifications** – Slack, Discord, and email notifications on specific events.
2. **Payment Processing** – Stripe, PayPal, and Razorpay send payment success/failure notifications.
3. **CI/CD Pipelines** – GitHub, GitLab, and Bitbucket trigger builds on new code commits.
4. **E-commerce & Order Management** – Shopify and WooCommerce notify systems of order status updates.
5. **CRM & Marketing Automation** – Zapier, HubSpot, and Salesforce trigger workflows based on customer actions.

### Pros of Webhooks

1. **Real-Time Updates** – Unlike REST polling, webhooks push data instantly.
2. **Efficient & Lightweight** – No need for repeated API requests, reducing server load.
3. **Easy to Implement** – Uses simple `POST` requests with JSON payloads.
4. **Great for Automation** – Enables seamless integration between services.
5. **Language-Agnostic** – Works with any tech stack that supports HTTP.

### Cons of Webhooks

1. **No Built-in Retry Mechanism** – If the client is down, the webhook request might be lost (unless handled manually).
2. **Security Risks** – Webhooks can be **spoofed** if not properly authenticated (e.g., using HMAC signatures).
3. **Difficult to Debug** – Since webhooks are event-driven, debugging failures can be tricky.
4. **Scalability Challenges** – High-frequency webhooks require a scalable infrastructure.
5. **No Response Guarantee** – The server doesn’t know if the client has successfully processed the data.

### Real-Life Examples of Webhooks

1. **GitHub & GitLab** – Trigger CI/CD workflows on code commits.
2. **Stripe & PayPal** – Notify merchants of successful payments.
3. **Slack & Discord Bots** – Send real-time messages to channels.
4. **Shopify & WooCommerce** – Update inventory and order details dynamically.
5. **Zapier & IFTTT** – Automate workflows between apps without polling.

# Comparison of API Protocols

| Feature | **SOAP** | **REST** | **GraphQL** | **gRPC** | **WebSocket** | **Webhook** |
| --- | --- | --- | --- | --- | --- | --- |
| **Definition** | XML-based protocol for structured messaging | Architectural style using HTTP for client-server communication | Query language for APIs enabling flexible data retrieval | High-performance RPC framework using Protobuf | Full-duplex protocol for real-time communication | Event-driven HTTP callbacks for notifications |
| **Communication Style** | Request-Response | Request-Response | Request-Response | Request-Response & Streaming | Persistent, Bidirectional | Server-initiated, One-way |
| **Data Format** | XML | JSON, XML, YAML, etc. | JSON (Protobuf possible) | Protobuf (binary) | JSON, Binary | JSON, XML |
| **Protocol Used** | HTTP, SMTP, TCP | HTTP | HTTP | HTTP/2 | TCP | HTTP |
| **Performance** | Slower due to XML overhead | Moderate | Moderate | High (binary serialization) | High (persistent connection) | High (event-driven) |
| **Best Use Cases** | Enterprise apps, banking, payment systems | Web & mobile APIs, microservices | Frontend-heavy apps, flexible data needs | Microservices, real-time streaming | Chat, gaming, live updates | Payment updates, CI/CD, notifications |
| **Caching Support** | Yes | Yes | No (workarounds exist) | No | No | No |
| **Security** | WS-Security (high) | OAuth, HTTPS, JWT | OAuth, HTTPS, JWT | TLS, OAuth, JWT | TLS (WSS required) | HMAC, Signature verification |
| **Streaming Support** | No | No | Yes (GraphQL Subscriptions) | Yes (Bidirectional Streaming) | Yes | No |
| **Ease of Use** | Complex | Easy | Moderate | Complex | Moderate | Easy |
| **Scalability** | Low | High | High | High | High | High |
| **Real-life Examples** | Financial transactions, legacy systems | Web APIs (Twitter, GitHub, Google Maps) | Facebook, GitHub, Shopify | Google Cloud, Netflix, Uber | WhatsApp, Slack, Multiplayer Games | Stripe, GitHub, Slack |

### Key Takeaways

1. **SOAP** is **best for enterprise applications** where strict security and reliability are needed.
2. **REST** is **widely used and flexible**, making it great for web and mobile APIs.
3. **GraphQL** is **ideal for frontend-heavy applications** needing precise data fetching.
4. **gRPC** is **best for high-performance, microservices, and inter-service communication**.
5. **WebSocket** is **perfect for real-time, bidirectional communication** (e.g., chat, gaming).
6. **Webhook** is **event-driven**, making it great for notifications, automation, and CI/CD pipelines.

# Deep Dive into RESTful APIs

A **RESTful API** (Representational State Transfer) is an architectural style that enables communication between clients and servers using HTTP methods. It is stateless, meaning each request is independent and does not rely on previous interactions. REST APIs are widely used due to their simplicity, scalability, and flexibility.

### Key Components of REST APIs - HTTP Methods (CRUD Operations)

| **HTTP Method** | **Action** | **Usage** |
| --- | --- | --- |
| `GET` | Read | Retrieve data from the server (e.g., `GET /users` for fetching all users). |
| `POST` | Create | Add new data to the server (e.g., `POST /users` to create a new user). |
| `PUT` | Update (Replace) | Modify an existing resource completely (e.g., `PUT /users/1` to replace user details). |
| `PATCH` | Update (Partial) | Modify part of an existing resource (e.g., `PATCH /users/1` to update only the email). |
| `DELETE` | Delete | Remove a resource from the server (e.g., `DELETE /users/1` to delete user 1). |

### REST API Request Structure

Every REST API request consists of three main parts:

1. **Query Parameters** (`?key=value`)
2. **Headers** (`key: value`)
3. **Body** (for `POST`, `PUT`, `PATCH` requests)

### Query Parameters (`?key=value`)

Query parameters are appended to the **URL** and are used to **filter, search, paginate, or customize** the response.

**Example:** `GET /products?category=electronics&limit=10&page=2`

| **Parameter** | **Purpose** |
| --- | --- |
| `category=electronics` | Filters products by category. |
| `limit=10` | Limits the number of results to 10. |
| `page=2` | Requests the second page of results. |

**When to use Query Parameters?**

- Filtering (`?status=active`)
- Sorting (`?sort=price_asc`)
- Pagination (`?page=3&limit=20`)
- Searching (`?query=laptop`)

### Headers (`key: value`)

Headers contain **metadata** about the request, such as authentication, content type, and caching instructions.

**Example:**

```
GET /users
Authorization: Bearer <token>
Accept: application/json
Content-Type: application/json
```

| **Header** | **Purpose** |
| --- | --- |
| `Authorization` | Used for authentication (e.g., `Bearer <token>`). |
| `Accept` | Specifies the expected response format (`application/json`, `text/xml`). |
| `Content-Type` | Specifies the request body format (`application/json`, `application/x-www-form-urlencoded`). |
| `Cache-Control` | Controls caching behavior (`no-cache`, `max-age=3600`). |

**When to use Headers?**

- **Authentication** (`Authorization: Bearer <token>`)
- **Content Negotiation** (`Accept: application/json`)
- **Security Measures** (`X-CSRF-Token: <token>`)
- **Custom API Key Authentication** (`X-API-Key: <key>`)

### Request Body (Payload)

The request **body** contains data that is sent to the server, mostly used in `POST`, `PUT`, and `PATCH` requests.

**Example:** Creating a new user:

```
POST /users
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "securePassword123"
}
```

**When to use Request Body?**

- **Sending complex data** (JSON objects, form data, file uploads)
- **Creating (`POST`) or Updating (`PUT`, `PATCH`) resources**
- **When data is too large for query parameters**

### REST API Response Structure

A REST API response typically consists of:

1. **Status Code** (Indicates success or failure)
2. **Headers** (Metadata about the response)
3. **Response Body** (Data returned from the server)

**Example: Successful Response (`200 OK`)**

```
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 1,
  "name": "John Doe",
  "email": "john@example.com"
}
```

**Common HTTP Status Codes:**

| **Code** | **Meaning** | **Example** |
| --- | --- | --- |
| `200 OK` | Request succeeded | `GET /users/1` returns user data. |
| `201 Created` | New resource created | `POST /users` successfully creates a user. |
| `204 No Content` | Successful request but no response body | `DELETE /users/1` deletes a user. |
| `400 Bad Request` | Invalid request syntax | Sending incorrect JSON format. |
| `401 Unauthorized` | Authentication failed | Missing or invalid API key/token. |
| `403 Forbidden` | Permission denied | User lacks access rights. |
| `404 Not Found` | Resource does not exist | `GET /users/999` for a non-existent user. |
| `500 Internal Server Error` | Server-side failure | Unhandled exception on the server. |

### Authentication in REST APIs

To secure REST APIs, authentication methods are used:

| **Method** | **How It Works** | **Use Cases** |
| --- | --- | --- |
| **API Key** | Sent in headers (`X-API-Key`) or query params (`?api_key=123`) | Public APIs, rate-limited endpoints |
| **Basic Auth** | Username & password encoded in Base64 (`Authorization: Basic <base64>`) | Simple authentication (not recommended) |
| **OAuth 2.0** | Token-based authentication (`Authorization: Bearer <token>`) | Secure, used by Google, Facebook |
| **JWT (JSON Web Token)** | Token-based, stateless authentication (`Authorization: Bearer <jwt_token>`) | User authentication, microservices |

### REST API Best Practices

1. **Use Nouns in URLs** – `GET /users` instead of `GET /getUsers`.
2. **Use Plural Resource Names** – `/users` instead of `/user`.
3. **Use HTTP Methods Correctly** – `GET` for fetching, `POST` for creating, etc.
4. **Return Proper Status Codes** – `404` for missing resources, `400` for bad requests.
5. **Version Your API** – `/api/v1/users` to allow future upgrades (`v2`, `v3`).
6. **Implement Pagination for Large Data** – `/users?page=2&limit=20`.
7. **Use Meaningful Error Messages** – `{ "error": "Invalid email format" }`.
8. **Secure APIs with Authentication** – Use `OAuth2`, `JWT`, or `API Keys`.
9. **Enable CORS for Cross-Origin Requests** – `Access-Control-Allow-Origin: *` for public APIs.
10. **Use Rate Limiting** – Prevent abuse by limiting requests per user/IP

### When NOT to Use REST APIs?

| **Problem** | **Alternative** |
| --- | --- |
| **Over-fetching or Under-fetching Data** | Use **GraphQL** for flexible queries. |
| **High Performance & Low Latency Needs** | Use **gRPC** for fast inter-service communication. |
| **Real-time, Persistent Connections** | Use **WebSockets** for chat, gaming, or live updates. |
| **Event-Driven Notifications** | Use **Webhooks** for pushing event data automatically. |

# Pagination in REST APIs

Pagination is a technique used to **split large datasets** into smaller, manageable chunks, making it easier to handle and more efficient for both the client and server. This is especially important in REST APIs when you have a large number of resources (e.g., users, products, posts) to return. There are multiple ways to implement pagination in RESTful APIs, but the most common techniques include **Offset-based pagination**, **Cursor-based pagination**, and **Page-based pagination**.

### Why Pagination is Needed

1. **Efficiency**: Returning large datasets in one go could overload the server or the client’s memory. Pagination allows the client to retrieve data in smaller portions.
2. **Improved Performance**: By limiting the data returned in each request, pagination reduces the time spent fetching resources and improves response time.
3. **User Experience**: For user-facing applications, pagination ensures that data is loaded progressively, leading to better UI/UX.

### Offset-Based Pagination

This is the **most common** pagination strategy, where the client specifies:

- **Page number**: The index of the page they want to fetch.
- **Limit**: The number of items to be returned per page (e.g., 10 items per page).

The URL structure typically looks like this:

```
GET /items?offset=20&limit=10
```

- **`offset=20`**: Start from the 21st item (skip the first 20 items).
- **`limit=10`**: Return 10 items.

The server returns the relevant data based on the offset and limit values, and the response might look like this:

```json
{
  "data": [
    { "id": 21, "name": "Item 21" },
    { "id": 22, "name": "Item 22" },
    { "id": 23, "name": "Item 23" },
    // ...
  ],
  "pagination": {
    "current_page": 3,
    "total_pages": 10,
    "total_items": 100
  }
}
```

- **Advantages**:
    - Simple and easy to implement.
    - Clients can jump to specific pages by changing the `offset` parameter.
- **Disadvantages**:
    - Inefficient for large datasets: As data grows, the offset value increases, leading to performance issues.
    - Data can change between requests (new data added, deleted, or updated), resulting in inconsistencies when navigating through pages.

### Cursor-Based Pagination

Cursor-based pagination uses a **unique identifier** (often called a cursor or a token) to point to the next batch of data rather than using a page number. This ensures more stable and efficient pagination, especially when data changes frequently.

The URL for cursor-based pagination often looks like this:

```
GET /items?limit=10&cursor=abc123
```

- **`limit=10`**: The number of items to return.
- **`cursor=abc123`**: A unique cursor token (usually the ID of the last item returned in the previous response).

In the response, the cursor is typically included in the body of the response:

```json
{
  "data": [
    { "id": 21, "name": "Item 21" },
    { "id": 22, "name": "Item 22" },
    { "id": 23, "name": "Item 23" },
    // ...
  ],
  "pagination": {
    "next_cursor": "xyz456"
  }
}
```

- **Advantages**:
    - More efficient for large datasets and real-time data because the cursor doesn’t change the offset.
    - **Stable**: Less affected by additions, deletions, or updates to the dataset.
- **Disadvantages**:
    - **No random access**: You can only move forward through the data.
    - More complex to implement than offset-based pagination.

### Page-Based Pagination

Page-based pagination allows clients to specify the page number and the number of items per page, similar to offset-based pagination, but it's handled slightly differently.

For example:

```
GET /items?page=3&per_page=10
```

- **`page=3`**: The client wants the third page of results.
- **`per_page=10`**: The client wants 10 items per page.

The server will return the corresponding results for page 3.

**Example Response:**

```json
{
  "data": [
    { "id": 21, "name": "Item 21" },
    { "id": 22, "name": "Item 22" },
    { "id": 23, "name": "Item 23" },
    // ...
  ],
  "pagination": {
    "current_page": 3,
    "total_pages": 10,
    "total_items": 100
  }
}
```

- **Advantages**:
    - Easy to understand and use.
    - Can be used with UI elements like "Previous" and "Next" buttons.
- **Disadvantages**:
    - Can suffer from the same inefficiencies as offset-based pagination when dealing with large datasets.

### Pagination Parameters

- **Limit**: The number of records to return per request (e.g., `limit=10`).
- **Offset**: The number of records to skip (e.g., `offset=20`).
- **Cursor**: A unique identifier pointing to the next set of data (used in cursor-based pagination).
- **Page Number**: The page number for pagination (e.g., `page=3`).
- **Per Page**: The number of items per page (e.g., `per_page=10`).

### Best Practices for Pagination in REST APIs

1. **Consistency**: Use consistent naming conventions for query parameters (e.g., `page`, `limit`, `cursor`).
2. **Provide Pagination Metadata**: Include pagination info (e.g., current page, total pages) in the response to help the client know where they are in the data.
3. **Limit the Number of Items Returned**: To avoid overloading the server, set sensible limits (e.g., `limit=50`).
4. **Support Both Forward and Backward Navigation** (for Cursor-Based Pagination): When using cursors, provide the cursor for both next and previous pages.
5. **Handle Edge Cases**: Make sure pagination works well when data changes, e.g., items being added or deleted between requests.
6. **Avoid Relying on Offsets for Critical Features**: Use cursor-based pagination for better consistency in dynamic datasets.

# Deep Dive into GraphQL APIs

GraphQL is a **query language** for APIs and a runtime for executing those queries by using a type system that you define for your data. It was developed by **Facebook** in 2012 and released as an open-source project in 2015. Unlike REST, which exposes multiple endpoints for different types of data and operations, GraphQL provides a **single endpoint** for all interactions, offering more flexibility and efficiency.

- **GraphQL** allows clients to request exactly the data they need, nothing more, nothing less.
- It enables **strongly typed schema-based queries**, meaning that the structure of the data (types, relationships, etc.) is predefined in the schema.
- **Single endpoint**: Unlike REST, which might require multiple endpoints (e.g., `/users`, `/posts`, `/comments`), GraphQL uses a **single endpoint** (e.g., `/graphql`) for all data requests.
- **Client-driven**: Clients define the structure of the response, requesting only the data they need.

### How Does GraphQL Work?

At the heart of **GraphQL** is the **Schema** and **Resolvers**:

### Schema

- The schema defines the **types of data** that the API can return and the **operations** that can be performed.
- It consists of:
    - **Types**: Define the shape of the data (e.g., `User`, `Post`).
    - **Queries**: Define the entry points for retrieving data.
    - **Mutations**: Define operations for creating, updating, or deleting data.
    - **Subscriptions**: Used for real-time communication (e.g., WebSocket-based subscriptions).

**Example of a simple GraphQL schema**:

```graphql
type Query {
  getUser(id: ID!): User
  getAllPosts: [Post]
}

type Mutation {
  createUser(name: String!): User
  updateUser(id: ID!, name: String!): User
}

type User {
  id: ID!
  name: String!
}

type Post {
  id: ID!
  title: String!
  content: String!
}
```

### Resolvers

- **Resolvers** are functions responsible for returning the data for a particular field in the schema. They act as the bridge between the schema and the data layer.
- For example, the resolver for the `getUser` query would retrieve the user data from a database or another service.

**Example of a resolver for `getUser`**:

```jsx
const resolvers = {
  Query: {
    getUser: (parent, args, context, info) => {
      return getUserById(args.id);  // Fetch user from DB
    }
  }
};
```

### GraphQL Query Structure

A **GraphQL query** specifies which data to retrieve. Clients request only the data they need.

**Example Query**:

```graphql
{
  getUser(id: "1") {
    name
    posts {
      title
      content
    }
  }
}
```

**Response**:

```json
{
  "data": {
    "getUser": {
      "name": "John Doe",
      "posts": [
        { "title": "Post 1", "content": "Content of post 1" },
        { "title": "Post 2", "content": "Content of post 2" }
      ]
    }
  }
}
```

### Client-Specified Queries (Key Feature of GraphQL)

In GraphQL, the client has complete control over the **structure** of the data. This means the client can request exactly the fields they need, avoiding over-fetching or under-fetching data.

- **Over-fetching**: With REST, you might retrieve more data than needed (e.g., getting 10 fields when you only need 3).
- **Under-fetching**: With REST, you may have to make multiple requests to get related data (e.g., fetching user data and then fetching the user’s posts in a separate request).

In GraphQL, you can specify exactly what you need:

**Example**:

```graphql
{
  user(id: 1) {
    name
    posts {
      title
    }
  }
}
```

**GraphQL Response**:

```json
{
  "data": {
    "user": {
      "name": "John Doe",
      "posts": [
        { "title": "First Post" },
        { "title": "Second Post" }
      ]
    }
  }
}
```

### Single Endpoint ****(Key Feature of GraphQL)

GraphQL uses a **single endpoint** (e.g., `/graphql`) for all requests, including queries, mutations, and subscriptions. This is in contrast to REST, which often requires a new endpoint for every type of resource and operation.

### Real-time Updates with Subscriptions (Key Feature of GraphQL)

GraphQL supports **subscriptions**, which allow the server to push updates to the client when certain events happen (e.g., when a new post is created or a user’s profile is updated).

**Example Subscription**:

```graphql
subscription {
  newPost {
    id
    title
    content
  }
}
```

Subscriptions are typically implemented using WebSockets for real-time data delivery.

### Advantages of GraphQL

1. **Efficient Data Fetching**: GraphQL allows clients to fetch only the data they need, preventing over-fetching and under-fetching of data.
2. **Single Endpoint**: Unlike REST, where you have multiple endpoints for different resources, GraphQL uses a single endpoint, simplifying API management.
3. **Strongly Typed Schema**: GraphQL provides a strongly typed schema that defines the data structure and operations, improving the maintainability of the API.
4. **Better Developer Experience**: GraphQL provides **introspection**, allowing developers to explore and query the schema to understand the available operations.
5. **Flexible and Scalable**: GraphQL can handle complex queries, including nested data and multiple operations in a single request. This can scale more effectively than REST in some scenarios.
6. **Versionless**: Because clients can specify the exact data they need, there’s no need for versioning the API as long as the schema remains backward compatible.

### Disadvantages of GraphQL

1. **Complexity**: GraphQL can be more complex to implement compared to REST, especially for teams unfamiliar with the technology. The resolver logic and schema management can become complicated with large datasets and many relationships.
2. **Overhead for Simple Use Cases**: For simple CRUD operations, REST may be more appropriate since GraphQL’s flexibility introduces overhead.
3. **Security**: Because clients can specify the queries they send, there’s a higher risk of **inefficient queries** (e.g., deeply nested queries that can strain the server). Proper query complexity analysis and rate limiting are required to avoid abuse.
4. **Caching**: Since all GraphQL requests go to the same endpoint, traditional HTTP caching mechanisms (like `GET` caching) do not work easily. You may need custom caching strategies.

### When to Use GraphQL vs REST

**GraphQL is ideal when:**

- **Clients need flexible data**: When the client needs to request only specific data or needs to work with highly dynamic data.
- **Multiple sources of data**: GraphQL is great when you need to pull data from multiple sources (e.g., databases, third-party services) into a single response.
- **Handling complex relationships**: If you need to manage complex relationships between resources (e.g., nested data like users with posts and comments).
- **Real-time data**: GraphQL subscriptions are perfect for use cases requiring real-time updates.

**REST is ideal when:**

- **Simple CRUD operations**: For simple, standard operations on resources where client flexibility is not a primary concern.
- **Established, legacy systems**: REST is a great choice for mature systems where the overhead of implementing GraphQL is unnecessary.
- **Cacheable responses**: REST APIs can take advantage of built-in HTTP caching.

### Best Practices for GraphQL APIs

- **Query Complexity Analysis**: Implement mechanisms to analyze the complexity of incoming queries and prevent abuse from deeply nested queries.
- **Rate Limiting**: Just like REST, GraphQL APIs need rate limiting to prevent overload.
- **Pagination**: Use `limit` and `offset` or `cursor-based pagination` to handle large datasets efficiently.
- **Versioning**: Although GraphQL is versionless, ensure backward compatibility and provide deprecation warnings if changes are made to the schema.
- **Schema First Development**: Build the schema first and then implement resolvers. This promotes a clear structure and a well-defined contract between clients and the server.
- **Error Handling**: Provide detailed and structured error messages, including error codes, descriptions, and possible fixes.
