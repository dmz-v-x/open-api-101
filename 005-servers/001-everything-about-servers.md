## Servers

### 1. Goal of this hands on step

You are going to **define where your API lives** by adding and evolving the `servers` section in a real OpenAPI file.

This is not deployment.
This is **environment awareness**.

By the end, you will clearly understand how tools, teams, and clients know *where* an API exists.

---

### 2. Start from a working base

Open your current `openapi.yaml`.

Make sure it looks like this before continuing:

    openapi: 3.0.3

    info:
      title: User Management API
      description: API for managing users, roles, and authentication across the platform
      version: 1.0.0

    paths:
      /health:
        get:
          summary: Health check
          responses:
            '200':
              description: API is healthy

    components: {}

This file is valid and ready.

---

### 4.1 What servers means

Add a `servers` section **below `info`**.

    servers:
      - url: https://api.example.com

Your file now tells tools:

This API is reachable at this base URL.

Hands on meaning:

- Tools prepend this URL to every path
- Generated clients use this base URL
- Docs show where requests go

Without `servers`, tools assume defaults.

With `servers`, location becomes explicit.

---

### 4.2 Single server vs multiple servers

Now add descriptions and a second server.

    servers:
      - url: https://api.example.com
        description: Production server

      - url: https://staging-api.example.com
        description: Staging server

Hands on meaning:

- Same API contract
- Multiple environments
- Same paths and schemas

Swagger UI will let users switch between servers.

One contract. Many locations.

---

### 4.3 Environment URLs dev staging prod

Now add a full environment setup.

    servers:
      - url: https://api.example.com
        description: Production

      - url: https://staging-api.example.com
        description: Staging

      - url: http://localhost:3000
        description: Local development

Hands on meaning:

- Frontend uses production
- QA uses staging
- Developers use localhost

All from one spec.

No environment guessing.

---

### 4.4 Server variables

Now replace hardcoded URLs with variables.

    servers:
      - url: https://{env}.api.example.com
        description: Environment-based server
        variables:
          env:
            default: prod
            enum:
              - prod
              - staging
              - dev

Hands on meaning:

- Base URL changes by variable
- Same structure reused
- Tools expose selectable environments

This reduces duplication and mistakes.

---

### 4.5 When NOT to hardcode environment URLs

Do NOT hardcode URLs when:

- You deploy per region
- You use customer-specific domains
- You have dynamic subdomains
- Infrastructure is ephemeral

Example avoided mistake:

    https://us-east-1.prod.api.example.com

Instead, use variables or leave servers flexible.

Hardcoding creates churn and broken specs.

---

### 6. Validate what you built

Open the spec in Swagger UI.

You should see:

- A server dropdown
- Environment switching
- Clean base URLs

No backend needed.
This is contract-level clarity.

---

### 7. Common beginner mistakes to avoid

Do NOT:

- Mix path and server URLs
- Put full URLs inside `paths`
- Forget descriptions
- Hardcode environment-specific chaos

Paths describe behavior.
Servers describe location.

Keep them separate.



Always hands on.
Always precise.
