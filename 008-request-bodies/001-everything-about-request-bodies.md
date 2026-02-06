## Request Bodies

### 1. Goal of this hands on step

You are going to **add real request bodies to real endpoints**.

Request bodies are how **structured data** enters your API.
This is where APIs stop being toy examples and start doing real work.

By the end, you will clearly know:

- When to use requestBody
- When NOT to use parameters
- How schemas define structure
- How validation is enabled by OpenAPI

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
        get:
          summary: List users
          operationId: listUsers
          responses:
            '200':
              description: List of users

        post:
          summary: Create user
          operationId: createUser
          responses:
            '201':
              description: User created

    components: {}

This is valid.
Now we will add a real request body.

---

### 7.1 What a request body is

A request body is:

- The main payload of a request
- Structured data
- Usually JSON
- Sent with POST, PUT, PATCH

Examples:

- Creating a user
- Updating a profile
- Submitting a form

If the input is **the thing being created or updated**, it belongs in the request body.

---

### 7.2 requestBody vs parameters

This distinction is critical.

Parameters are for:

- Identifiers
- Filters
- Flags
- Metadata

Request bodies are for:

- Objects
- Structured data
- Business payloads

Wrong approach:

    POST /users?name=Alice&email=a@example.com

Correct approach:

    POST /users
    { "name": "Alice", "email": "a@example.com" }

OpenAPI enforces this separation.

---

### 7.3 Add a requestBody with content type

Update the `post` operation like this:

    post:
      summary: Create user
      operationId: createUser
      requestBody:
        content:
          application/json:
            schema:
              type: object
      responses:
        '201':
          description: User created

Hands on meaning:

- requestBody defines payload
- content defines media type
- application/json is explicit

Without content type, the spec is invalid.

---

### 7.4 Define a schema for the request body

Now define the actual structure.

Replace the schema with this:

    requestBody:
      content:
        application/json:
          schema:
            type: object
            properties:
              name:
                type: string
              email:
                type: string
                format: email
              age:
                type: integer
                minimum: 0
            required:
              - name
              - email

Hands on meaning:

- properties define allowed fields
- required defines mandatory fields
- validation rules are explicit

This describes exactly what the API accepts.

---

### 7.5 Required vs optional request bodies

By default, request bodies are optional.

Now make it required.

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

Hands on meaning:

- Client must send a body
- POST without a body is invalid
- Tools enforce this expectation

For POST and PUT, request bodies are usually required.

---

### 7.6 Multiple content types

APIs can accept more than one content type.

Example:

    requestBody:
      required: true
      content:
        application/json:
          schema:
            type: object
            properties:
              name:
                type: string

        application/xml:
          schema:
            type: object

Hands on meaning:

- Same operation
- Multiple representations
- Tools let clients choose

Most APIs stick to JSON.
But OpenAPI supports more.

---

### 7.7 Examples for request bodies

Now add an example.

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
          example:
            name: Alice
            email: alice@example.com

Hands on meaning:

- Examples help humans
- Shown in Swagger UI
- Do not affect validation

Examples explain intent.
Schemas enforce rules.

---

### 7.8 Validation mindset OpenAPI enables it

Important mindset shift:

OpenAPI does NOT validate requests by itself.

What it does:

- Defines what is allowed
- Defines structure
- Defines rules

Validation happens when:

- Middleware uses the spec
- CI checks responses
- Tools compare runtime vs contract

OpenAPI enables validation.
It does not magically enforce it.

---

### 7.9 Common beginner mistakes to avoid

Do NOT:

- Put request bodies in query parameters
- Omit content types
- Forget schemas
- Use requestBody with GET
- Treat examples as validation

Very common mistake:

    GET /users
    { "page": 1 }

GET requests do not have request bodies.
Use query parameters instead.

---

### 8. Validate what you built

Open the spec in Swagger UI.

You should see:

- POST /users
- JSON body editor
- Required fields marked
- Example pre-filled

No backend needed.
This is contract-driven development.

---

### 9. Mental model to lock in

- parameters = small inputs
- requestBody = main payload
- content = media type
- schema = structure and rules
- examples = documentation only

If input is structured, it belongs in requestBody.
