## Responses

### 1. Goal of this hands on step

You are going to **define real API responses** and understand why responses are the **most critical part of an API contract**.

Clients do not care how your API is implemented.
They care only about **what comes back**.

By the end, you will understand how to model outputs so clients can safely depend on them.

---

### 2. Start from a working base

Open your `openapi.yaml` and start from this working file:

    openapi: 3.0.3

    info:
      title: User Management API
      version: 1.0.0

    servers:
      - url: http://localhost:3000

    paths:
      /users:
        post:
          summary: Create user
          operationId: createUser
          requestBody:
            required: true
            content:
              application/json:
                schema:
                  type: object
                  properties:
                    name:
                      type: string
                    email:
                      type: string
                  required:
                    - name
                    - email
          responses: {}

    components: {}

This file is valid but incomplete.
Now we will define outputs.

---

### 8.1 What a response object is

A response object describes:

- What the server sends back
- For a specific HTTP status code
- Including headers and body

Every operation **must** have at least one response.

If responses are wrong or vague, clients break.

---

### 8.2 HTTP status codes as keys

Responses are defined using HTTP status codes as keys.

Add a success response:

    responses:
      '201':
        description: User created

Hands on meaning:

- Keys are strings, not numbers
- Each key represents a possible outcome
- Tools match runtime responses to these keys

Without a status code, the operation is invalid.

---

### 8.3 Response description

Now focus on this line:

    description: User created

Hands on meaning:

- Description is mandatory
- Explains what the response means
- Shown in documentation

This is not optional.
Even empty responses need a description.

---

### 8.4 Response headers

Now add a response header.

    responses:
      '201':
        description: User created
        headers:
          Location:
            description: URL of the created user
            schema:
              type: string

Hands on meaning:

- Headers are part of the contract
- Clients may depend on them
- Headers must also have schemas

This models:

    Location: /users/123

---

### 8.5 Response body schemas

Now add a response body.

    responses:
      '201':
        description: User created
        headers:
          Location:
            description: URL of the created user
            schema:
              type: string
        content:
          application/json:
            schema:
              type: object
              properties:
                id:
                  type: string
                name:
                  type: string
                email:
                  type: string

Hands on meaning:

- content defines media type
- schema defines response structure
- This is the **output contract**

Clients generate models from this schema.

---

### 8.6 Multiple responses per endpoint

Now add an error response.

    responses:
      '201':
        description: User created
        content:
          application/json:
            schema:
              type: object
              properties:
                id:
                  type: string
                name:
                  type: string
                email:
                  type: string

      '400':
        description: Invalid request
        content:
          application/json:
            schema:
              type: object
              properties:
                message:
                  type: string
                code:
                  type: string

Hands on meaning:

- One operation can have many outcomes
- Each outcome must be documented
- Clients branch logic based on status codes

Never document only the happy path.

---

### 8.7 Default responses

Now add a default response.

    responses:
      '201':
        description: User created

      '400':
        description: Invalid request

      default:
        description: Unexpected error
        content:
          application/json:
            schema:
              type: object
              properties:
                message:
                  type: string

Hands on meaning:

- `default` catches undocumented errors
- Acts as a safety net
- Still must have a description

This is useful for unexpected failures.

---

### 8.8 Error responses consistency

Now step back and look at error responses.

Bad practice:

- Different error shapes everywhere
- Inconsistent fields
- Guessing on the client side

Good practice:

- One consistent error schema
- Same structure across endpoints
- Reused via components

Example consistent error shape:

    {
      "code": "INVALID_INPUT",
      "message": "Email is required"
    }

Clients depend on this consistency.

---

### 9. Validate what you built

Open the spec in Swagger UI.

You should see:

- Multiple responses per endpoint
- Status codes clearly listed
- Response schemas rendered
- Headers documented

All without backend code.

---

### 10. Mental model to lock in

- responses = output contract
- status code = outcome
- description = meaning
- headers = metadata
- schema = structure
- multiple responses = real-world behavior

Clients trust responses more than anything else.

---

### 11. Common beginner mistakes to avoid

Do NOT:

- Omit error responses
- Return undocumented status codes
- Change response shapes casually
- Use vague descriptions
- Return different error formats per endpoint

Response changes are breaking changes.
