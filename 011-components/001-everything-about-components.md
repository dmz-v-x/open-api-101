## Components

### 1. Goal of this hands on step

You are going to **refactor a real OpenAPI spec** to use `components` properly.

This is where OpenAPI starts to **scale**.
Without components, large specs collapse into duplication chaos.

By the end, you will understand how reuse works and how tools resolve it.

---

### 2. Start from a duplicated spec on purpose

Open your `openapi.yaml` and start from this intentionally repetitive version:

    openapi: 3.0.3

    info:
      title: User Management API
      version: 1.0.0

    servers:
      - url: http://localhost:3000

    paths:
      /users/{id}:
        get:
          summary: Get user
          operationId: getUser
          parameters:
            - name: id
              in: path
              required: true
              schema:
                type: string
          responses:
            '200':
              description: User details
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

      /users:
        get:
          summary: List users
          operationId: listUsers
          responses:
            '200':
              description: Users list
              content:
                application/json:
                  schema:
                    type: array
                    items:
                      type: object
                      properties:
                        id:
                          type: string
                        name:
                          type: string
                        email:
                          type: string

    components: {}

This works.
But it is already repeating itself.

---

### 10.1 Why reuse matters

Duplication causes:

- Inconsistent schemas
- Forgotten updates
- Breaking changes
- Painful reviews

If the same data appears twice, it **will drift**.

Components exist to stop this.

---

### 10.2 components.schemas

Move the user schema into components.

Add this:

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

Now update both responses.

    schema:
      $ref: '#/components/schemas/User'

And for the list:

    schema:
      type: array
      items:
        $ref: '#/components/schemas/User'

Now the schema exists once.
Used everywhere.

---

### 10.3 components.parameters

Now reuse the `id` parameter.

Add:

    components:
      parameters:
        UserId:
          name: id
          in: path
          required: true
          schema:
            type: string

Update the path:

    parameters:
      - $ref: '#/components/parameters/UserId'

Now the path parameter is defined once.

---

### 10.4 components.responses

Now extract a common response.

Add:

    components:
      responses:
        UserResponse:
          description: User details
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'

Use it:

    responses:
      '200':
        $ref: '#/components/responses/UserResponse'

This removes duplication and enforces consistency.

---

### 10.5 $ref syntax

The `$ref` syntax always looks like this:

    $ref: '#/components/{section}/{Name}'

Important rules:

- `$ref` replaces the entire object
- You cannot add siblings next to `$ref`
- Indentation must be exact

Wrong:

    schema:
      $ref: '#/components/schemas/User'
      nullable: true

Correct:

    schema:
      allOf:
        - $ref: '#/components/schemas/User'
        - nullable: true

---

### 10.6 Avoiding duplication mindset

Before adding anything, ask:

- Is this already defined?
- Will this be reused?
- Will this change later?

If yes, it belongs in `components`.

Duplication is technical debt in disguise.

---

### 10.7 Circular references warning

Be very careful with circular references.

Example dangerous pattern:

- User references Organization
- Organization references User

This can:

- Break generators
- Cause infinite loops
- Crash documentation tools

Circular references are allowed but dangerous.

Use them only when absolutely necessary.

---

### 11. Validate the refactored spec

Open the spec in Swagger UI.

You should see:

- Same API behavior
- Cleaner spec
- Centralized schemas
- No duplication

Same output.
Better structure.

---

### 12. Mental model to lock in

- components = reusable building blocks
- schemas = data models
- parameters = shared inputs
- responses = shared outputs
- $ref = pointer, not copy

Reuse is not optional at scale.
