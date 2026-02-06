## Schemas

### 1. Goal of this hands on step

You are going to **build real schemas** and understand why schemas are the **most reused and most important part** of OpenAPI.

Schemas define the **shape of data**.
Everything else references them.

By the end, you will be comfortable reading and writing schemas without guessing.

---

### 2. Start from a working base

Open your `openapi.yaml` and start from this file:

    openapi: 3.0.3

    info:
      title: User Management API
      version: 1.0.0

    servers:
      - url: http://localhost:3000

    paths: {}

    components:
      schemas: {}

This is valid and ready.
All schemas will live under `components.schemas`.

---

### 9.1 What a schema is

A schema describes **the shape of data**.

It answers questions like:

- What fields exist
- What types they are
- Which are required
- What values are allowed

Schemas are used for:

- Request bodies
- Response bodies
- Parameters
- Errors

If data exists, a schema should describe it.

---

### 9.2 JSON Schema basics OpenAPI subset

OpenAPI schemas are based on **JSON Schema**, but not the full spec.

Key idea:

- OpenAPI uses a subset
- Enough to describe API data
- Focused on validation and structure

Everything you define is about **shape**, not behavior.

---

### 9.3 Primitive types

Start with the simplest schema.

Add this under `components.schemas`:

    components:
      schemas:
        UserId:
          type: string

This defines:

- A value
- That must be a string

Other primitive types you can use:

    type: string
    type: number
    type: integer
    type: boolean

Primitive schemas are the building blocks.

---

### 9.4 Objects and properties

Now define a real object.

    components:
      schemas:
        User:
          type: object
          properties:
            id:
              type: string
            name:
              type: string
            email:
              type: string

Hands on meaning:

- type: object means JSON object
- properties define allowed fields
- Fields not listed are not guaranteed

At this stage, all fields are optional.

---

### 9.5 Required fields

Now make some fields mandatory.

    components:
      schemas:
        User:
          type: object
          properties:
            id:
              type: string
            name:
              type: string
            email:
              type: string
          required:
            - id
            - name
            - email

Hands on meaning:

- required is a list
- Only applies to object properties
- Missing required fields are invalid

Required fields are critical for clients.

---

### 9.6 Arrays of objects

Now define a list of users.

    components:
      schemas:
        UserList:
          type: array
          items:
            $ref: '#/components/schemas/User'

Hands on meaning:

- type: array means JSON array
- items defines element shape
- Reuses the User schema

This models:

    [
      { "id": "1", "name": "A", "email": "a@x.com" },
      { "id": "2", "name": "B", "email": "b@x.com" }
    ]

---

### 9.7 Enums

Now restrict allowed values.

    components:
      schemas:
        UserRole:
          type: string
          enum:
            - admin
            - member
            - guest

Hands on meaning:

- Value must be one of these
- Anything else is invalid
- Clients can generate safe types

Enums remove ambiguity.

---

### 9.8 Nullable fields

Now allow a field to be null.

    components:
      schemas:
        User:
          type: object
          properties:
            id:
              type: string
            name:
              type: string
            middleName:
              type: string
              nullable: true
            email:
              type: string
          required:
            - id
            - name
            - email

Hands on meaning:

- Field may be string or null
- Different from optional
- Optional = field missing
- Nullable = field present but null

This distinction matters for clients.

---

### 9.9 Formats

Formats add **semantic meaning** to primitives.

Update the schema:

    components:
      schemas:
        User:
          type: object
          properties:
            id:
              type: string
              format: uuid
            name:
              type: string
            email:
              type: string
              format: email
            createdAt:
              type: string
              format: date-time
          required:
            - id
            - name
            - email

Common formats:

- email
- date-time
- uuid
- uri

Formats help tooling and validation.

---

### 9.10 Schema examples

Now add an example.

    components:
      schemas:
        User:
          type: object
          properties:
            id:
              type: string
              format: uuid
            name:
              type: string
            email:
              type: string
              format: email
          required:
            - id
            - name
            - email
          example:
            id: "550e8400-e29b-41d4-a716-446655440000"
            name: Alice
            email: alice@example.com

Hands on meaning:

- Examples help humans
- Shown in documentation
- Do not enforce validation

Schemas enforce rules.
Examples explain intent.

---

### 10. Use the schema in a response

Now actually use your schema.

    paths:
      /users/{id}:
        get:
          summary: Get user
          operationId: getUser
          responses:
            '200':
              description: User details
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/User'

This is real reuse.

One schema.
Many endpoints.

---

### 11. Common beginner mistakes to avoid

Do NOT:

- Redefine the same object everywhere
- Skip required fields
- Use vague schemas like type: object only
- Forget array item schemas
- Confuse nullable with optional

Schemas should be precise.

---

### 12. Mental model to lock in

- schema = data shape
- primitives = building blocks
- objects = structured data
- arrays = collections
- required = must exist
- nullable = may be null
- formats = semantic hints

If data is shared, it belongs in a schema.
