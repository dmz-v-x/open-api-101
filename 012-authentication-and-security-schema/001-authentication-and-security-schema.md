## Authentication & Security Schema

### 1. Goal of this hands on step

You are going to **add real authentication definitions** to a real OpenAPI spec.

Security is not an afterthought.
It is part of the **API contract**.

By the end, you will know:

- How APIs declare authentication
- How clients know what to send
- How tools enforce security expectations

No backend required.
Pure contract work.

---

### 2. Start from a working base

Open your `openapi.yaml` and start from this file:

    openapi: 3.0.3

    info:
      title: User Management API
      version: 1.0.0

    servers:
      - url: http://localhost:3000

    paths:
      /users:
        get:
          summary: List users
          operationId: listUsers
          responses:
            '200':
              description: List of users

    components: {}

This is valid.
Now we add security.

---

### 1. Why security is part of the contract

Security answers critical questions:

- How does a client authenticate
- What credentials are required
- Which endpoints are protected

If this is undocumented:

- Clients guess
- Integrations break
- Security bugs appear

Security must be **explicit**.

---

### 2. securitySchemes overview

All security definitions live under:

    components:
      securitySchemes:

This section defines **how authentication works**.

Important rule:

- Defining a scheme does NOT apply it
- Application comes later

First, we define mechanisms.

---

### 3. API Key authentication

Add an API key scheme.

    components:
      securitySchemes:
        ApiKeyAuth:
          type: apiKey
          in: header
          name: X-API-Key

Hands on meaning:

- Client sends a key
- Key lives in a header
- Header name is explicit

This maps to:

    X-API-Key: abc123

Common API key locations:

- header
- query
- cookie

Header is preferred.

---

### 4. HTTP authentication Basic and Bearer

Now add HTTP auth schemes.

    components:
      securitySchemes:
        BasicAuth:
          type: http
          scheme: basic

        BearerAuth:
          type: http
          scheme: bearer

Hands on meaning:

- BasicAuth → username:password
- BearerAuth → token-based auth

Bearer is the foundation for JWT.

---

### 5. JWT bearer tokens

JWT is not a separate OpenAPI type.

It is a **Bearer token with a format hint**.

Update BearerAuth:

    BearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT

Hands on meaning:

- Clients know token type
- Docs show JWT explicitly
- Tools generate correct headers

This maps to:

    Authorization: Bearer eyJhbGciOiJIUzI1NiIs...

---

### 6. OAuth2 flows high level

OAuth2 is more complex, but OpenAPI supports it.

Add an OAuth2 scheme:

    components:
      securitySchemes:
        OAuth2:
          type: oauth2
          flows:
            authorizationCode:
              authorizationUrl: https://auth.example.com/oauth/authorize
              tokenUrl: https://auth.example.com/oauth/token
              scopes:
                read:users: Read user data
                write:users: Modify user data

Hands on meaning:

- Defines OAuth2 behavior
- Declares scopes
- Tools can generate auth UIs

OpenAPI does NOT implement OAuth.
It only describes it.

---

### 7. Applying security globally

Now apply security to the entire API.

Add this at the root level:

    security:
      - BearerAuth: []

Hands on meaning:

- All endpoints require BearerAuth
- Empty array means no scopes required
- Clients must send Authorization header

This is the most common setup.

---

### 8. Applying security per route

Now override security for a specific endpoint.

Example: public health check.

    /health:
      get:
        summary: Health check
        security: []
        responses:
          '200':
            description: OK

Hands on meaning:

- Empty security array disables auth
- This endpoint is public
- Global security is overridden

You can also mix schemes:

    security:
      - ApiKeyAuth: []
      - BearerAuth: []

This means **either** is accepted.

---

### 9. Validate what you built

Open the spec in Swagger UI.

You should see:

- Lock icons on secured endpoints
- Authorize button
- Token input fields
- Correct headers auto-injected

No backend required.
This is contract-driven security.

---

### 10. Common beginner mistakes to avoid

Do NOT:

- Hardcode auth headers in examples
- Forget to apply security
- Mix auth logic into descriptions
- Assume tools infer auth automatically

Security must be explicit.

---

### 11 Mental model to lock in

- securitySchemes = how auth works
- security = where auth applies
- definition ≠ application
- OpenAPI describes, not enforces

If clients do not know how to authenticate, the API is broken.
