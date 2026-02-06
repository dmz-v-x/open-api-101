## Examples & Documentation

### 1. Goal of this hands on step

You are going to **add real examples** to your OpenAPI spec and see how they dramatically improve **developer experience**.

Examples are what developers actually read first.
Schemas are rules.
Examples are understanding.

By the end, you will know how to use examples without turning them into lies.

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
        post:
          summary: Create user
          operationId: createUser
          requestBody:
            required: true
            content:
              application/json:
                schema:
                  $ref: '#/components/schemas/CreateUser'
          responses:
            '201':
              description: User created
              content:
                application/json:
                  schema:
                    $ref: '#/components/schemas/User'

    components:
      schemas:
        CreateUser:
          type: object
          properties:
            name:
              type: string
            email:
              type: string
          required:
            - name
            - email

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
          required:
            - id
            - name
            - email

This works.
Now we will make it friendly.

---

### 12.1 Why examples matter more than descriptions

Descriptions explain intent.
Examples explain reality.

Developers usually:

- Scan schemas briefly
- Copy examples immediately

If examples are missing or fake:

- Integration slows down
- Bugs increase
- Trust drops

Good examples reduce questions more than any paragraph.

---

### 12.2 Request examples

Add an example to the request body.

Update `CreateUser`:

    components:
      schemas:
        CreateUser:
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

- Example shows valid input
- Matches the schema exactly
- Used by Swagger UI to prefill requests

This instantly tells developers how to call the API.

---

### 12.3 Response examples

Now add an example to the response schema.

Update `User`:

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
          required:
            - id
            - name
            - email
          example:
            id: "550e8400-e29b-41d4-a716-446655440000"
            name: Alice
            email: alice@example.com

Hands on meaning:

- Shows real response shape
- Matches actual API output
- Helps frontend model state correctly

Clients depend on response examples heavily.

---

### 12.4 Multiple examples per schema

Sometimes one example is not enough.

Add multiple examples at the **media type level**.

Update the response like this:

    responses:
      '201':
        description: User created
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/User'
            examples:
              success:
                summary: Successful creation
                value:
                  id: "550e8400-e29b-41d4-a716-446655440000"
                  name: Alice
                  email: alice@example.com

              anotherUser:
                summary: Another valid user
                value:
                  id: "2f1c2e90-0a2d-4c3a-9c8e-8e0b0f5c1234"
                  name: Bob
                  email: bob@example.com

Hands on meaning:

- Multiple real-world scenarios
- Developers can switch examples
- Still validated by the same schema

Use multiple examples sparingly and intentionally.

---

### 12.5 Keeping examples realistic

Bad examples cause real damage.

Avoid examples like:

    id: "123"
    email: "test@test"

Instead:

- Use realistic UUIDs
- Use believable names
- Use valid formats
- Match production behavior

Rules to follow:

- Examples must always validate against schemas
- Examples must reflect real API behavior
- Never use placeholder junk

Examples are part of trust.

---

### 12.6 Examples vs mocks

Important distinction:

- Examples explain
- Mocks simulate

Examples:

- Live in OpenAPI
- Used for understanding
- Used in docs

Mocks:

- Use OpenAPI
- Return fake responses
- Used for development and testing

Do not confuse them.

Examples are documentation.
Mocks are tools.

---

### 13. Validate what you built

Open the spec in Swagger UI.

You should see:

- Request body pre-filled
- Response examples visible
- Multiple example selection
- Clear, realistic payloads

This dramatically improves DX.

---

### 14. Common beginner mistakes to avoid

Do NOT:

- Let examples drift from schemas
- Use unrealistic placeholder values
- Forget to update examples after changes
- Put logic explanations inside examples

Examples must be correct, not clever.

---

### 15. Mental model to lock in

- Schemas = rules
- Examples = understanding
- Descriptions = intent
- Examples must follow schemas
- Good examples reduce questions

If developers are confused, your examples are failing.


