# Authentication & Authorisation

- [Cookies](#cookies)
    - [What Are Cookies?](#what-are-cookies)
    - [How Cookies Work (Step by Step)](#how-cookies-work-step-by-step)
    - [Types of Cookies](#types-of-cookies)
    - [Uses of Cookies](#uses-of-cookies)
    - [**Privacy & Security Concerns**](#privacy--security-concerns)
- [Session (ID-based Authentication)](#session-id-based-authentication)
    - [What is Session (ID-based Authentication)?](#what-is-session-id-based-authentication)
    - [How Session ID-based Authentication Works (Step-by-Step)](#how-session-id-based-authentication-works-step-by-step)
    - [Where is the Session ID Stored?](#where-is-the-session-id-stored)
- [JWT (Token-based Authentication)](#jwt-token-based-authentication)
    - [What is JWT (Token-Based Authentication)?](#what-is-jwt-token-based-authentication)
    - [How JWT-based Authentication Works (Step-by-Step)](#how-jwt-based-authentication-works-step-by-step)
    - [JWT Structure](#jwt-structure)
    - [**Where is JWT Stored?**](#where-is-jwt-stored)
- [Session vs JWT: Key Differences](#session-vs-jwt-key-differences)
    - [When to Use What?](#when-to-use-what)
- [Different type of tokens](#different-type-of-tokens)
    - [What are ID Token, Access Token, and Refresh Token?](#what-are-id-token-access-token-and-refresh-token)
    - [Comparison Table](#comparison-table)
- [OAuth 2.0 & OpenID Connect (OIDC)](#oauth-20--openid-connect-oidc)
    - [What is OAuth 2.0?](#what-is-oauth-20)
    - [How OAuth 2.0 Works (Step-by-Step)](#how-oauth-20-works-step-by-step)
    - [What is OpenID Connect (OIDC)?](#what-is-openid-connect-oidc)
    - [How OpenID Connect (OIDC) Works (Step-by-Step)](#how-openid-connect-oidc-works-step-by-step)
    - [Key Components of OIDC](#key-components-of-oidc)
    - [How OIDC Differs from OAuth 2.0?](#how-oidc-differs-from-oauth-20)
    - [When to Use OAuth 2.0 & OpenID Connect (OIDC)?](#when-to-use-oauth-20--openid-connect-oidc)
    - [Examples of When to Use OAuth 2.0](#examples-of-when-to-use-oauth-20)
    - [Examples of When to Use OpenID Connect (OIDC)](#examples-of-when-to-use-openid-connect-oidc)
- [SAML](#saml)
    - [What is SAML (Security Assertion Markup Language)?](#what-is-saml-security-assertion-markup-language)
    - [How SAML Works (Step-by-Step)](#how-saml-works-step-by-step)
    - [Key Components of SAML](#key-components-of-saml)
    - [When to Use SAML?](#when-to-use-saml)
- [Single Sign-on (SSO)](#single-sign-on-sso)
    - [What is SSO (Single Sign-On)?](#what-is-sso-single-sign-on)
    - [How SSO Works (Step-by-Step)](#how-sso-works-step-by-step)
    - [SSO Can Be Implemented Using SAML or OIDC](#sso-can-be-implemented-using-saml-or-oidc)
    - [When to Use SAML vs. OIDC for SSO?](#when-to-use-saml-vs-oidc-for-sso)
- [LDAP](#ldap)
    - [What is LDAP (Lightweight Directory Access Protocol)?](#what-is-ldap-lightweight-directory-access-protocol)
    - [How LDAP Works (Step-by-Step)](#how-ldap-works-step-by-step)
    - [Key Components of LDAP](#key-components-of-ldap)
    - [When to Use LDAP?](#when-to-use-ldap)
    - [Difference Between LDAP, SAML, and OIDC](#difference-between-ldap-saml-and-oidc)
- [Authentication vs. Authorization](#authentication-vs-authorization)
    - [Authentication (Who Are You?)](#authentication-who-are-you)
    - [Authorization (What Can You Do?)](#authorization-what-can-you-do)
    - [Key Differences: Authentication vs. Authorization](#key-differences-authentication-vs-authorization)
    - [Analogy](#analogy)
- [Summary](#summary)

# Cookies

[**What cookies are and how they work!**](https://youtu.be/s04Vjlcgwco)

[**What Are Cookies? And How They Work**](https://youtu.be/rdVPflECed8)

### What Are Cookies?

Cookies are small text files stored on a user's browser by websites to remember information about the user. They help improve the browsing experience and enable functionalities like authentication, session management, and tracking.

### How Cookies Work (Step by Step)

1. **User Visits a Website:** When a user visits a website, the server may send a cookie to the user's browser.
2. **Browser Stores the Cookie:** The browser saves the cookie and associates it with the website.
3. **Sending Cookies with Requests:** When the user revisits the site, the browser automatically sends the cookie back to the server.
4. **Website Reads the Cookie:** The website retrieves the stored cookie and uses the data to personalize content, authenticate users, or track activity.

### Types of Cookies

1. **Session Cookies** – Temporary cookies that expire when the browser is closed.
2. **Persistent Cookies** – Stored on the device for a specified period, even after closing the browser.
3. **First-Party Cookies** – Set by the website being visited.
4. **Third-Party Cookies** – Set by domains other than the website being visited (e.g., for ads and tracking).
5. **Secure Cookies** – Only sent over HTTPS for security.
6. **HttpOnly Cookies** – Cannot be accessed by JavaScript to prevent cross-site scripting (XSS) attacks.

### Uses of Cookies

1. **Authentication:** Keeps users logged in.
2. **Session Management:** Stores shopping cart items, form data, etc.
3. **Personalization:** Remembers user preferences, like language settings.
4. **Tracking & Analytics:** Helps websites understand user behavior.

### **Privacy & Security Concerns**

1. **Tracking & Privacy:** Advertisers use third-party cookies for user tracking.
2. **Data Leaks:** If not secured, cookies can be stolen (session hijacking).
3. **Regulations:** GDPR, CCPA, and other laws require user consent for cookies.

# Session (ID-based Authentication)

### What is Session (ID-based Authentication)?

Session-based authentication is a **stateful authentication method** where the server maintains user sessions using a unique **Session ID**. This ID is stored on the client and sent with each request to identify the user.

### How Session ID-based Authentication Works (Step-by-Step)

1. **User Logs In** – The user provides credentials (username & password).
2. **Server Verifies Credentials** – The server checks if the credentials are valid.
3. **Session Created** – If valid, the server creates a **Session ID** and stores it in a session store (memory, database, Redis, etc.).
4. **Session ID Sent to Client** – The Session ID is sent back to the client in a **cookie** (or another storage mechanism).
5. **Client Sends Session ID on Requests** – Every subsequent request includes the Session ID (usually in cookies).
6. **Server Validates Session** – The server checks if the Session ID is still valid.
7. **Access Granted or Denied** – If the Session ID is valid, the user gets access; otherwise, they are redirected to log in.
8. **Session Expiry or Logout** – The session can expire after a timeout or be invalidated when the user logs out.

### Where is the Session ID Stored?

1. **On the Client:** In a browser cookie (often HttpOnly & Secure).
2. **On the Server:** In a session store (in-memory, database, Redis, etc.).

# JWT (Token-based Authentication)

[**Why is JWT popular?**](https://youtu.be/P2CPd9ynFLg)

[**What is JWT token and JWT vs Sessions**](https://youtu.be/xrj3zzaqODw)

### What is JWT (Token-Based Authentication)?

JWT (**JSON Web Token**) is a compact, self-contained token used for **stateless authentication**. Instead of storing user sessions on the server, the server issues a **signed token** that the client stores and sends with every request.

### How JWT-based Authentication Works (Step-by-Step)

1. **User Logs In** – The user submits valid credentials (username & password).
2. **Server Verifies Credentials** – If valid, the server generates a **JWT token**.
3. **JWT Sent to Client** – The server sends the JWT to the client in the response.
4. **Client Stores JWT** – The client saves the JWT in **localStorage, sessionStorage, or cookies**.
5. **Client Sends JWT on Requests** – Every request includes the JWT in the **Authorization header** (`Bearer <token>`).
6. **Server Verifies JWT** – The server checks the JWT’s **signature, expiration, and claims**.
7. **Access Granted or Denied** – If valid, access is granted; otherwise, authentication fails.
8. **Token Expiry or Logout** – The JWT expires after a set time, or the user logs out (requiring a new login).

### JWT Structure

A JWT consists of three parts:

1. **Header** – Specifies token type (`JWT`) and signing algorithm (`HS256`, `RS256`, etc.).
2. **Payload** – Contains claims (user ID, roles, expiration time, etc.).
3. **Signature** – A cryptographic signature that ensures token integrity.

JWT Example (Base64 Encoded):

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.
eyJ1c2VySWQiOiIxMjM0Iiwicm9sZSI6ImFkbWluIiwiZXhwIjoxNjk0MDAwMDAwfQ.
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

### **Where is JWT Stored?**

1. **Cookies** – More secure if `HttpOnly` & `Secure` flags are set.
2. **localStorage/sessionStorage** – Convenient but vulnerable to XSS attacks.
3. **Authorization Header** – Best for API authentication (`Bearer <token>`).

# Session vs JWT: Key Differences

[**Session Vs JWT: The Differences You May Not Know!**](https://youtu.be/fyTxwIa-1U0)

[**Difference between cookies, session and tokens**](https://youtu.be/GhrvZ5nUWNg)

| Feature | **Session (ID-based Authentication)** | **JWT (Token-based Authentication)** |
| --- | --- | --- |
| **Storage** | Stored on the server (session store, database, or memory). | Stored on the client (localStorage, sessionStorage, or cookie). |
| **Structure** | Random string (session ID). | Self-contained JSON object (Header, Payload, Signature). |
| **Authentication** | Server verifies session ID by looking it up in storage. | Server validates JWT using a cryptographic signature. |
| **Stateful/Stateless** | **Stateful** (Server must track active sessions). | **Stateless** (No need to store session data on the server). |
| **Scalability** | Difficult to scale (requires centralized session storage). | Highly scalable (no need for a shared session store). |
| **Security Risks** | Session hijacking if session ID is exposed. | Token theft (if stored in localStorage) and replay attacks. |
| **Performance** | Requires DB/memory lookup for each request. | Faster since no lookup is needed (signature verification only). |
| **Expiration** | Manually managed by the server. | Tokens have built-in expiration (`exp` claim). |
| **Logout Handling** | Server deletes session from storage. | Cannot be revoked unless stored in a database (blacklisting required). |

### When to Use What?

1. **Use Sessions when:**
    - You need strict control over logins & logout.
    - Your app is small/medium-scale and session storage isn't a bottleneck.
    - Security is a high concern (sessions are easier to revoke).
2. **Use JWT when:**
    - You need a stateless and scalable system (e.g., microservices, APIs).
    - Your authentication needs to work across multiple domains/services.
    - You need an access/refresh token mechanism (OAuth, Single Sign-On).

# Different type of tokens

[**ID Tokens VS Access Tokens: What's the Difference?**](https://youtu.be/vVM1Tpu9QB4)

[**What is a Refresh Token and why your REST API needs it?**](https://youtu.be/-Z57Ss_uiuc)

### What are ID Token, Access Token, and Refresh Token?

1. **ID Token**
    - **What it is:** A JWT (JSON Web Token) that contains identity-related information about the user.
    - **When it is used:** Used during authentication to confirm a user's identity.
    - **What it is used for:** Helps client applications verify that a user is authenticated (e.g., in Single Sign-On or OpenID Connect flows).
2. **Access Token**
    - **What it is:** A token (JWT or opaque) that grants permission to access protected resources (e.g., APIs).
    - **When it is used:** Used in authorization flows after authentication.
    - **What it is used for:** Sent in API requests to prove the user's authorization to access specific resources (e.g., fetching user profile data, accessing account details).
3. **Refresh Token**
    - **What it is:** A long-lived token used to obtain a new access token without requiring user reauthentication.
    - **When it is used:** Used when the access token expires.
    - **What it is used for:** Maintains user sessions without requiring the user to log in again (used in OAuth 2.0 flows).

### Comparison Table

| Feature | **ID Token** | **Access Token** | **Refresh Token** |
| --- | --- | --- | --- |
| **Purpose** | User authentication (who the user is) | Grants access to resources | Issues a new access token when expired |
| **Usage** | Used by client apps to verify user identity | Sent with API requests to authenticate | Sent to auth server to get a new access token |
| **Issued By** | Authentication server (e.g., OpenID Connect) | Authentication server | Authentication server |
| **Contains** | User details (name, email, roles, etc.) | Permissions & scopes (what the user can do) | Metadata, expiry info |
| **Format** | JWT | JWT or opaque token | JWT or opaque token |
| **Expiration** | Short-lived | Short-lived | Long-lived |
| **Stored In** | Client-side (browser, app) | Client-side (header, cookie) | Secure storage (server-side or HttpOnly cookie) |
| **Security Risk** | Should not be used for authorization | Can be stolen if not secured properly | If stolen, attacker can generate new tokens |
| **Used In** | OpenID Connect (OIDC), Single Sign-On (SSO) | OAuth 2.0, API requests | OAuth 2.0, token refresh mechanism |

# **OAuth 2.0 & OpenID Connect (OIDC)**

[**An Illustrated Guide to OAuth and OpenID Connect**](https://youtu.be/t18YB3xDfXI)

[**OAuth 2.0 and OpenID Connect (in plain English)**](https://youtu.be/996OiexHze0)

[**What is OAuth and why does it matter?**](https://youtu.be/KT8ybowdyr0)

[**OAuth and OpenID Connect - Know the Difference**](https://youtu.be/HsbNDDfLvio)

### What is OAuth 2.0?

OAuth 2.0 is an **authorization framework** that allows third-party applications to access a user's resources **without exposing their credentials**. It is widely used for API authentication and delegated access (e.g., logging in with Google/Facebook).

### How OAuth 2.0 Works (Step-by-Step)

1. **User Requests Access** – The user wants to log in or authorize an application.
2. **Client Redirects User to Authorization Server** – The application (client) directs the user to the OAuth provider (e.g., Google, Facebook).
3. **User Grants Permission** – The user consents to allow the application to access specific data.
4. **Authorization Server Issues Authorization Code** – If the user approves, the provider sends a temporary **authorization code** to the client.
5. **Client Requests Access Token** – The client exchanges the authorization code for an **access token** by sending it to the provider’s token endpoint.
6. **Authorization Server Issues Access Token** – The provider validates the request and returns an **access token** (and optionally a **refresh token**).
7. **Client Uses Access Token** – The application includes the access token in API requests to retrieve user data or perform actions.
8. **Token Expiry & Refresh Token Usage** – If the access token expires, the client can use the **refresh token** to obtain a new one without requiring the user to log in again.

### What is OpenID Connect (OIDC)?

OpenID Connect (**OIDC**) is an **authentication layer** built on top of OAuth 2.0. It enables users to log in using an **identity provider (IdP)** (e.g., Google, Facebook) and provides an **ID Token** to verify their identity.

### How OpenID Connect (OIDC) Works (Step-by-Step)

1. **User Requests Authentication** – The user wants to log in to an application using an external provider (e.g., "Login with Google").
2. **Client Redirects User to Identity Provider (IdP)** – The application (client) redirects the user to the OIDC provider (e.g., Google, Facebook).
3. **User Logs In & Grants Consent** – The user enters their credentials and approves the requested permissions.
4. **IdP Issues Authorization Code** – After successful login, the provider sends an **authorization code** back to the client.
5. **Client Requests Tokens** – The client exchanges the authorization code for an **ID Token**, **Access Token**, and optionally a **Refresh Token** from the IdP’s token endpoint.
6. **Client Verifies ID Token** – The client decodes the **ID Token** (a JWT) to extract user identity details (e.g., name, email).
7. **User is Authenticated** – The client grants access based on the verified identity.
8. **Access Token Used for APIs** – The **Access Token** is used for authorization when calling protected APIs.
9. **Token Refresh (if needed)** – If the **Access Token** expires, the **Refresh Token** is used to obtain a new one without requiring the user to log in again.

### Key Components of OIDC

1. **ID Token** – A JWT containing user identity details (e.g., name, email, profile picture).
2. **Access Token** – Grants access to protected APIs (like OAuth 2.0).
3. **Refresh Token** – Used to get a new Access Token without user reauthentication.
4. **UserInfo Endpoint** – An API that provides additional user details.

### How OIDC Differs from OAuth 2.0?

| Feature | **OAuth 2.0** | **OIDC (OpenID Connect)** |
| --- | --- | --- |
| **Purpose** | Authorization (grants access to resources) | Authentication (verifies user identity) |
| **Primary Token** | Access Token | ID Token (JWT) |
| **User Info** | Not included by default | Contains user identity (name, email, etc.) |
| **Used In** | API access (e.g., Google Drive API) | Login systems (e.g., "Sign in with Google") |

### When to Use OAuth 2.0 & OpenID Connect (OIDC)?

- **Use OAuth 2.0** when you need **authorization** (access control to APIs & resources).
- **Use OpenID Connect (OIDC)** when you need **authentication** (verifying who the user is).

### Examples of When to Use OAuth 2.0

1. **Third-Party API Access** – A weather app wants to access a user’s Google Calendar events.
2. **Mobile App Authorization** – A fitness tracking app requests access to a user's Steps data.
3. **Machine-to-Machine Authentication** – A backend service needs to call an external payment API securely.
4. **Cloud Storage Access** – A web app wants to retrieve files from the user's Dropbox account.

✅ **OAuth 2.0 is best for:** API security, delegated access, and third-party service authorization.

### Examples of When to Use OpenID Connect (OIDC)

1. **Single Sign-On (SSO)** – A corporate intranet allows employees to log in using their Google or Microsoft account.
2. **"Login with Google/Facebook"** – A shopping website enables users to log in using Google instead of creating a new account.
3. **Enterprise Authentication** – A company uses an Identity Provider (IdP) like Okta or Auth0 to manage logins for multiple applications.
4. **Banking & Healthcare Authentication** – A healthcare portal verifies a user’s identity using a government-issued OIDC provider.

✅ **OIDC is best for:** Secure user authentication, identity management, and Single Sign-On (SSO).

# **SAML**

[**What is SAML?**](https://youtu.be/4ULlJEupV-I)

### What is SAML (Security Assertion Markup Language)?

SAML is an **XML-based authentication and authorization standard** used for **Single Sign-On (SSO)**. It enables users to log in once and access multiple applications securely without re-entering credentials.

### How SAML Works (Step-by-Step)

1. **User Requests Access** – The user tries to access a service provider (SP) (e.g., a cloud app like Salesforce).
2. **SP Redirects User to Identity Provider (IdP)** – The SP redirects the user to the IdP (e.g., Okta, Azure AD) for authentication.
3. **User Authenticates with IdP** – The user enters their credentials on the IdP's login page.
4. **IdP Generates a SAML Assertion** – If authentication is successful, the IdP creates a **SAML assertion** (an XML document) containing user identity and permissions.
5. **SAML Assertion Sent to SP** – The IdP redirects the user back to the SP with the **SAML assertion**.
6. **SP Validates the SAML Assertion** – The SP verifies the assertion's signature and extracts user identity details.
7. **User is Logged In** – If valid, the SP grants access to the user.

### Key Components of SAML

1. **Identity Provider (IdP)** – The service that authenticates users (e.g., Okta, Google, Azure AD).
2. **Service Provider (SP)** – The application that users want to access (e.g., Salesforce, Slack).
3. **SAML Assertion** – A secure XML document containing user identity and permissions.
4. **SAML Binding** – The method used to transmit SAML data (e.g., HTTP Redirect, HTTP POST).

### **When to Use SAML?**

1. **Enterprise SSO** – Used for **corporate logins** (e.g., logging into multiple business apps with one account).
2. **Cloud-Based Authentication** – Allows users to log in to **third-party services** (e.g., AWS, Google Workspace).
3. **Federated Identity Management** – Enables **cross-domain authentication** without sharing passwords.

# **Single Sign-on (SSO)**

[**What Is Single Sign-on (SSO)? How It Works**](https://youtu.be/O1cRJWYF-g4)

[**Build Your Own SSO | What is SSO | SSO Explained**](https://youtu.be/JuyaVlK-kGQ)

### What is SSO (Single Sign-On)?

**Single Sign-On (SSO)** is an **authentication mechanism** that allows users to log in once and access multiple applications **without re-entering credentials**.

### How SSO Works (Step-by-Step)

1. **User Requests Access** – The user tries to access an application (Service Provider - SP).
2. **Application Redirects to Identity Provider (IdP)** – The app redirects the user to an authentication provider (e.g., Okta, Google, Azure AD).
3. **User Authenticates with IdP** – The user logs in with their credentials at the IdP.
4. **IdP Issues Authentication Token** – If successful, the IdP generates a **token/assertion** containing the user's identity.
5. **Token Sent to Application (SP)** – The IdP sends the authentication token back to the SP.
6. **Application Grants Access** – The application validates the token and logs in the user **without requiring another password**.
7. **SSO Session Maintained** – The user can now access other connected apps **without re-authenticating**.

### SSO Can Be Implemented Using SAML or OIDC

| Feature | **SAML (Security Assertion Markup Language)** | **OIDC (OpenID Connect)** |
| --- | --- | --- |
| **Purpose** | Authentication & SSO for enterprise apps | Authentication & SSO for modern web/mobile apps |
| **Token Format** | **SAML Assertion** (XML-based) | **ID Token** (JWT-based) |
| **Best For** | **Enterprise SSO** (e.g., corporate apps, Microsoft 365, Salesforce) | **Web & Mobile SSO** (e.g., "Login with Google", social logins) |
| **Complexity** | More complex (XML-based, requires more setup) | Simpler (JSON-based, widely supported) |
| **Authentication Flow** | Redirects via browser to IdP | Uses OAuth 2.0 flows with tokens |
| **Flexibility** | Primarily for enterprise applications | Works for consumer & enterprise apps |
| **Common Providers** | Okta, Azure AD, OneLogin, Ping Identity | Google, Facebook, Auth0, AWS Cognito |

### When to Use SAML vs. OIDC for SSO?

1. **Use SAML** if your organization uses enterprise applications like **Microsoft 365, Salesforce, or AWS** and needs **corporate SSO**.
2. **Use OIDC** for modern applications, **web/mobile logins**, and **social authentication** (e.g., "Sign in with Google/Facebook").

# LDAP

### What is LDAP (Lightweight Directory Access Protocol)?

LDAP is a **protocol** used to access and manage directory information services, such as user authentication, permissions, and organizational data. It is commonly used in **corporate networks** for **centralized authentication and directory management**.

### How LDAP Works (Step-by-Step)

1. **Client Requests Authentication** – A user or application tries to log in or retrieve information.
2. **Client Connects to LDAP Server** – The request is sent to an **LDAP Server (Directory Server)**, such as **Active Directory (AD), OpenLDAP, or Apache Directory Server**.
3. **LDAP Server Searches the Directory** – The server looks up the requested data (e.g., username, email, group membership).
4. **Server Verifies Credentials** – If authentication is requested, the server checks the **username and password** against stored records.
5. **Response Sent to Client** – If authentication is successful, the user is granted access. If querying information, the requested data is returned.
6. **Client Accesses Resources** – Once authenticated, the user can access company systems, emails, applications, etc.

### Key Components of LDAP

1. **LDAP Server (Directory Server)** – Stores user and system data (e.g., Microsoft Active Directory).
2. **LDAP Client** – Applications that send authentication or data requests to the LDAP server.
3. **Directory Information Tree (DIT)** – A hierarchical database structure storing users, groups, devices, etc.
4. **Distinguished Name (DN)** – A unique identifier for each entry in the LDAP directory.
5. **Bind Operation** – The process of authenticating a user.

### When to Use LDAP?

1. **Enterprise Authentication** – Used in large organizations for managing user logins.
2. **Centralized Directory Services** – Maintains a **single source of truth** for users, groups, and devices.
3. **Application Integration** – Many applications (e.g., Jenkins, GitLab) can authenticate users via LDAP.

### Difference Between LDAP, SAML, and OIDC

| Feature | **LDAP (Lightweight Directory Access Protocol)** | **SAML (Security Assertion Markup Language)** | **OIDC (OpenID Connect)** |
| --- | --- | --- | --- |
| **Purpose** | Centralized authentication & directory management | Authentication & SSO for enterprise applications | Authentication & SSO for modern web/mobile apps |
| **Type** | Protocol for querying user information | Authentication standard (SSO) | Authentication layer on top of OAuth 2.0 |
| **Data Format** | Plain-text-based | XML-based | JSON Web Token (JWT) |
| **Best For** | On-premise user directories & authentication (Active Directory, OpenLDAP) | Enterprise SSO (corporate apps like Salesforce, Microsoft 365) | Web & mobile authentication (Google, Facebook logins) |
| **How It Works** | Applications query the directory for user credentials and permissions | The IdP provides a SAML assertion (XML token) to authenticate users | The IdP provides an ID Token (JWT) for authentication |
| **SSO Support** | Can be used for authentication but **not designed for SSO** | Yes, widely used for enterprise SSO | Yes, best for modern web & mobile SSO |
| **Security** | Basic authentication with username/password | Strong authentication with digital signatures | Strong authentication with OAuth 2.0 security |
| **Common Providers** | Microsoft Active Directory, OpenLDAP | Okta, Azure AD, OneLogin, Ping Identity | Google, Auth0, AWS Cognito |

# Authentication vs. Authorization

Authentication and Authorization are two fundamental concepts in security that determine **who can access a system and what they can do** once inside.

### Authentication (Who Are You?)

1. **Definition**: Authentication is the process of **verifying a user's identity** before granting access.
2. **How It Works**:
    - The user provides **credentials** (e.g., username/password, biometrics, tokens).
    - The system **validates** these credentials.
    - If valid, the user is **authenticated** and granted access.
3. **Examples**:
    - Logging into a website with **username & password**.
    - Using **Face ID** or **fingerprint scan** on a smartphone.
    - **"Sign in with Google/Facebook"** using OpenID Connect (OIDC).
    - **Multi-Factor Authentication (MFA)** for extra security (e.g., OTPs).
4. **Verifies user identity** before granting access.
    - **LDAP** – Used for user authentication in enterprise directories.
    - **OIDC (OpenID Connect)** – Authentication layer on top of OAuth 2.0 (used in web/mobile login).
    - **SAML** – Used for Single Sign-On (SSO) in enterprise authentication.

### Authorization (What Can You Do?)

1. **Definition**: Authorization is the process of **determining what actions a user is allowed to perform** after authentication.
2. **How It Works**:
    - Once authenticated, the system checks the **user’s roles & permissions**.
    - It enforces **access control rules** to restrict or allow actions.
3. **Examples**:
    - A **regular user** can only view their profile, but an **admin** can edit all profiles.
    - In a banking app, a **customer** can view their account, but an **employee** can access multiple accounts.
    - OAuth 2.0 grants an **access token** to limit what data a third-party app can use.
4. **Controls user permissions** after authentication.
    - **OAuth 2.0** – Grants access tokens to allow specific actions (e.g., accessing an API).
    - **SAML & OIDC** – Provide identity details that applications use for authorization decisions.

### Key Differences: Authentication vs. Authorization

| Feature | **Authentication** | **Authorization** |
| --- | --- | --- |
| **Purpose** | Verifies **who** you are | Determines **what** you can do |
| **Process** | Identity validation (login) | Permission & access control |
| **Example** | Logging in with username & password | Restricting admin-only features |
| **Technology Used** | LDAP, OIDC, SAML, MFA | OAuth 2.0, Role-Based Access Control (RBAC) |

### Analogy

- **Authentication** = Showing your ID at the building entrance to prove who you are.
- **Authorization** = Security deciding whether you can enter specific rooms inside the building.

# Summary

We explored key concepts in **Authentication & Authorization**, which are critical for securing modern applications.

**Key Takeaways:**

1. **Authentication (Who Are You?)** – Verifies a user's identity using **LDAP, SAML, OIDC**, and other methods.
2. **Authorization (What Can You Do?)** – Controls user access using **OAuth 2.0, Role-Based Access Control (RBAC), and Access Tokens**.
3. **SSO (Single Sign-On)** – Allows users to log in once and access multiple apps using **SAML or OIDC**.
4. **Token Types** – **ID Token (OIDC), Access Token (OAuth 2.0), and Refresh Token** for session management.
5. **LDAP vs. SAML vs. OIDC** – LDAP is used for **on-premise authentication**, while **SAML & OIDC enable SSO for enterprise and web apps**.

**Final Thoughts:**

- **Use SAML** for **enterprise SSO** (corporate apps like Microsoft 365).
- **Use OIDC** for **modern authentication** (Google login, mobile apps).
- **Use OAuth 2.0** for **secure API access** (third-party integrations).
- **Use LDAP** for **on-premise directory authentication** (Active Directory).

Useful flow diagram: [https://drive.google.com/drive/folders/109OtDAFPAqr0SgT5DJAyLKKCCEi_WMIL](https://drive.google.com/drive/folders/109OtDAFPAqr0SgT5DJAyLKKCCEi_WMIL?usp=sharing)
