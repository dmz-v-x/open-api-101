## Minimal Valid OpenAPI Document

### 1. Goal of this hands on step

You are going to **create a real OpenAPI file** and understand its root structure by actually seeing it work.

No theory first.
We start with a real file and build understanding from it.

By the end of this step, you will clearly understand these four root keys:

openapi  
info  
paths  
components  

---

### 2. Step one create the file

Create a new file on your machine:

openapi.yaml

This file is the OpenAPI specification.
Everything starts here.

---

### 3. Step two write the smallest valid OpenAPI file

Paste this into `openapi.yaml` exactly as written.

    openapi: 3.0.3

    info:
      title: Sample API
      version: 1.0.0

    paths: {}

This is a **valid OpenAPI 3.x file**.

Even though it does almost nothing, tools will accept it.

This file already shows three root keys.

---

### 4. Root key one openapi

Look at this line:

    openapi: 3.0.3

Hands on meaning:

- This tells tools which OpenAPI rules to use
- It must be OpenAPI 3.x
- Without this, nothing works

Important rules:

- This is NOT your API version
- This is the OpenAPI specification version

Think of it as:

“I am speaking OpenAPI 3.0.3 language”

---

### 5. Root key two info

Now look at this block:

    info:
      title: Sample API
      version: 1.0.0

Hands on meaning:

- This describes the API itself
- Humans see this in generated docs
- Tools require it

Minimum required fields inside info:

- title
- version

This version **is your API version**, not OpenAPI’s version.

You can already open this file in Swagger UI and see a titled API.

---

### 6. Root key three paths

Now focus on this:

    paths: {}

Hands on meaning:

- paths is where all endpoints live
- Every REST endpoint goes here
- No paths means no API behavior described

Right now it is empty, so the API has no endpoints.

This is normal at the beginning.

---

### 7. Add your first real endpoint hands on

Replace the `paths` section with this:

    paths:
      /health:
        get:
          summary: Health check
          responses:
            '200':
              description: API is healthy

Now your file describes a real endpoint.

What you just did:

- Defined a URL
- Defined an HTTP method
- Defined a response

This is real OpenAPI work.

---

### 8. Root key four components

Now add the fourth root key at the bottom:

    components: {}

So the full file now looks like:

    openapi: 3.0.3

    info:
      title: Sample API
      version: 1.0.0

    paths:
      /health:
        get:
          summary: Health check
          responses:
            '200':
              description: API is healthy

    components: {}

Hands on meaning of components:

- This is where reusable pieces live
- Schemas
- Responses
- Parameters
- Security definitions

You do not need it on day one.
But real APIs always grow into it.

---

### 9. Mental model you should lock in now

These four root keys have **clear roles**:

- openapi  
  Defines the spec language

- info  
  Describes the API for humans

- paths  
  Describes API behavior and endpoints

- components  
  Stores reusable building blocks

If you understand this, you understand the backbone of OpenAPI.

---

### 10. Verify your work visually

Open this file in:

- Swagger UI
- Swagger Editor
- Any OpenAPI viewer

You should see:

- API title
- Version
- One endpoint: GET /health

No backend required.
No server required.

---

### 11. Common beginner mistake to avoid

Do NOT:

- Skip components thinking it is optional forever
- Put schemas directly inside paths everywhere
- Treat info as documentation only

This structure exists to scale cleanly.
