## Parameters

### 1. Goal of this hands on step

You are going to **add real parameters to real endpoints** and see exactly how inputs flow into an API.

Parameters are where most beginners make mistakes.
So we will build them carefully, break them, and fix them.

By the end, you will clearly know **what belongs in parameters and what does not**.

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

      /users/{id}:
        get:
          summary: Get user by ID
          operationId: getUserById
          responses:
            '200':
              description: User details

    components: {}

This is valid.
Now we will add inputs.

---

### 6.1 What parameters are

Parameters are **inputs sent with the request** that are:

- Small
- Scalar
- Part of the request metadata

They are NOT:

- Large payloads
- JSON bodies
- Complex objects

If it is not the main payload, it is usually a parameter.

---

### 6.2 Parameter locations

OpenAPI supports exactly four parameter locations:

- path
- query
- header
- cookie

Nothing else.

Let us add each one hands on.

---

### 6.3 Path parameters users id

Your path already has `{id}`.

Now you must define it.

Update `/users/{id}` like this:

    /users/{id}:
      get:
        summary: Get user by ID
        operationId: getUserById
        parameters:
          - name: id
            in: path
            required: true
            schema:
              type: string
        responses:
          '200':
            description: User details

Hands on meaning:

- name must match `{id}`
- in must be path
- required must be true
- schema defines the type

If any of these are wrong, the spec is invalid.

---

### 6.4 Path parameters must be required

This rule is non-negotiable.

Try this experiment.

Change:

    required: true

To:

    required: false

Reload in Swagger Editor.

Result:

- Validation error

Why:

- A URL cannot exist without its path value
- `/users/{id}` without `id` makes no sense

OpenAPI enforces this strictly.

---

### 6.5 Query parameters pagination example

Now add query parameters to `GET /users`.

    /users:
      get:
        summary: List users
        operationId: listUsers
        parameters:
          - name: page
            in: query
            required: false
            schema:
              type: integer
              minimum: 1

          - name: pageSize
            in: query
            required: false
            schema:
              type: integer
              minimum: 1
              maximum: 100
        responses:
          '200':
            description: List of users

Hands on meaning:

- Query params filter or modify behavior
- They are optional by default
- Schema controls validation

This maps to:

    GET /users?page=1&pageSize=20

---

### 6.6 Header parameters

Now add a header parameter.

    parameters:
      - name: X-Request-Id
        in: header
        required: false
        schema:
          type: string

Hands on meaning:

- Headers carry metadata
- Common for tracing, auth, versioning
- Do NOT put business data here

Never redefine standard headers like `Authorization` manually unless required.

---

### 6.7 Cookie parameters

Now add a cookie parameter example.

    parameters:
      - name: session_id
        in: cookie
        required: false
        schema:
          type: string

Hands on meaning:

- Rare but supported
- Mostly used in browser-based APIs
- Still explicit in OpenAPI

---

### 6.8 Parameter schemas

Every parameter MUST have a schema.

This is invalid:

    schema: {}

This is valid:

    schema:
      type: string

Schemas define:

- Type
- Format
- Constraints
- Validation rules

Without schema, tools cannot validate inputs.

---

### 6.9 Reusing parameters via components

Now move a parameter into `components`.

Add this:

    components:
      parameters:
        UserId:
          name: id
          in: path
          required: true
          schema:
            type: string

Now update the path to use `$ref`:

    parameters:
      - $ref: '#/components/parameters/UserId'

Hands on meaning:

- One definition
- Reused everywhere
- Consistency guaranteed

This is critical for large APIs.

---

### 6.10 Common beginner mistakes to avoid

Do NOT:

- Put JSON bodies in query params
- Send complex objects as query strings
- Forget schemas
- Forget required on path params
- Duplicate parameters everywhere

Very common wrong example:

    GET /users?user={ "id": "123", "name": "A" }

This is not REST.
This is not OpenAPI-friendly.

Use request bodies for payloads.

---

### 7. Validate what you built

Open the spec in Swagger UI.

You should see:

- Path parameter input for id
- Query inputs for pagination
- Clear required vs optional markers
- No validation errors

All without writing backend code.

---

### 8. Mental model to lock in

- Parameters = request metadata
- path = identifies resource
- query = modifies behavior
- header = protocol metadata
- cookie = browser context
- schema = validation rule

If input is complex, it is NOT a parameter.
