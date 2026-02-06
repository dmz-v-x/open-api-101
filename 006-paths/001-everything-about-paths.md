## Paths

### 1. Goal of this hands on step

You are going to work with the **most important section of OpenAPI**: `paths`.

This is where your API actually comes to life.

Every REST endpoint you have ever used maps directly to this section.

By the end, you will be able to read and write `paths` confidently and understand how tools see your API.

---

### 2. Start from a clean working base

Open your `openapi.yaml` and start from this base:

    openapi: 3.0.3

    info:
      title: User Management API
      version: 1.0.0

    servers:
      - url: http://localhost:3000
        description: Local development

    paths: {}

    components: {}

This file is valid and ready.

---

### 5.1 What paths represents

The `paths` object represents **all the URLs your API exposes**.

Think of `paths` as:

- The API surface
- The REST contract
- The list of everything clients can call

If it is not inside `paths`, it does not exist as an API endpoint.

---

### 5.2 Path keys users users id

Now add your first real path.

Replace `paths: {}` with:

    paths:
      /users: {}

Hands on meaning:

- `/users` is a URL path
- It maps directly to `/users` in REST
- No HTTP methods yet, so nothing is callable

Paths are just containers until methods are added.

---

### 5.3 HTTP methods under paths

Now add a `get` method under `/users`.

    paths:
      /users:
        get:
          responses:
            '200':
              description: List of users

Hands on meaning:

- `/users` is the resource
- `get` is the HTTP action
- This describes `GET /users`

Every HTTP method is written in lowercase:

- get
- post
- put
- patch
- delete

Uppercase is invalid in OpenAPI.

---

### 5.4 Operation objects one HTTP action

Each HTTP method is called an **operation**.

This block is one operation:

    get:
      responses:
        '200':
          description: List of users

An operation represents:

- One HTTP method
- On one path
- With one behavior

So:

GET /users  
POST /users  
GET /users/{id}  

Each one is a **separate operation**.

---

### 5.5 summary vs description

Now add `summary` and `description`.

    paths:
      /users:
        get:
          summary: List users
          description: Returns a paginated list of all users in the system
          responses:
            '200':
              description: List of users

Hands on difference:

- `summary`  
  Short  
  One-line  
  Used in lists and navigation  

- `description`  
  Longer  
  Detailed  
  Explains behavior and context  

Swagger UI shows summary first, description on expand.

---

### 5.6 operationId very important for codegen

Now add an `operationId`.

    paths:
      /users:
        get:
          summary: List users
          description: Returns a paginated list of all users in the system
          operationId: listUsers
          responses:
            '200':
              description: List of users

Hands on meaning:

- `operationId` is a **unique name** for this operation
- Client SDKs use it as method names
- Code generators rely heavily on it

Example generated code:

    client.listUsers()

Rules you must follow:

- Must be unique across the entire API
- Use clear verb-based naming
- Never leave it out in real APIs

---

### 5.7 Path parameters users id

Now add another path using a parameter.

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

Hands on meaning:

- `{id}` is a path parameter
- This maps directly to REST URLs like `/users/123`
- Curly braces are mandatory

The parameter definition itself will come later.
For now, this defines the shape.

---

### 5.8 Tags for grouping endpoints

Now add tags to group operations.

    paths:
      /users:
        get:
          tags:
            - Users
          summary: List users
          operationId: listUsers
          responses:
            '200':
              description: List of users

      /users/{id}:
        get:
          tags:
            - Users
          summary: Get user by ID
          operationId: getUserById
          responses:
            '200':
              description: User details

Hands on meaning:

- Tags group endpoints in documentation
- Swagger UI uses them as sections
- Tags have no runtime effect

Without tags, large APIs become unreadable.

---

### 6. Validate what you built

Open this file in Swagger UI.

You should see:

- Users section
- Two endpoints
- Clear summaries
- Clean grouping
- No errors

No backend needed.
This is pure contract work.

---

### 7. Common beginner mistakes to avoid

Do NOT:

- Put full URLs inside paths
- Use uppercase HTTP methods
- Skip operationId
- Reuse operationId
- Mix unrelated endpoints under random tags

Paths must stay clean and predictable.

---

### 8. Mental model you should lock in

- paths = API surface
- path key = URL
- HTTP method = action
- operation = one REST call
- operationId = code identity
- tags = documentation grouping

If you understand this, you understand REST in OpenAPI.



