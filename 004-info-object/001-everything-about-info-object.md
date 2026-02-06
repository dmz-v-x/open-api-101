## Info Object

### 1. Goal of this hands on step

You are going to **build a real `info` object step by step** and understand why it represents the **identity of the API**.

This is not decoration.
This is how teams, tools, and integrations recognize your API.

By the end, you will know exactly **who this API is** just by reading `info`.

---

### 2. Start with a minimal working base

Open your existing `openapi.yaml` and make sure you start from this working base:

    openapi: 3.0.3

    info:
      title: Demo API
      version: 1.0.0

    paths: {}

This is valid.
Now we will grow the `info` object safely.

---

### 3. What the info object represents

The `info` object answers one simple question:

Who is this API?

It defines:

- Name
- Purpose
- Versioning
- Ownership
- Legal context

Every human and tool reads this first.

---

### 3.1 title naming the API

The `title` is the **official name** of your API.

Update your file:

    info:
      title: User Management API
      version: 1.0.0

Hands on meaning:

- Appears as the main heading in docs
- Used by client generators
- Used by teams to reference the API

Rules to follow:

- Be specific
- Avoid internal codenames
- Treat it like a product name

Bad example:
“Backend API”

Good example:
“User Management API”

---

### 3.2 description what problem it solves

Now add a description.

    info:
      title: User Management API
      description: API for managing users, roles, and authentication across the platform
      version: 1.0.0

Hands on meaning:

- Explains purpose, not implementation
- Read by frontend, mobile, and partners
- Sets expectations

What to include:

- What problem this API solves
- What it does not do
- High-level scope

What to avoid:

- Database details
- Internal architecture
- Step-by-step usage

---

### 3.3 version API version vs implementation version

This is a **very important distinction**.

Your file already has:

    version: 1.0.0

This version means:

- API contract version
- What clients depend on
- What breaking changes affect

It does NOT mean:

- Git commit
- Build number
- Deployment version

Hands on rule:

Change this version ONLY when the contract changes.

Not when code is refactored.

---

### 3.4 termsOfService

Now add terms of service.

    info:
      title: User Management API
      description: API for managing users, roles, and authentication across the platform
      version: 1.0.0
      termsOfService: https://example.com/terms

Hands on meaning:

- Legal usage terms
- Especially important for public APIs
- Shown clearly in generated docs

Even internal APIs benefit from this clarity.

---

### 3.5 contact info

Now define who owns this API.

Add a `contact` block:

    info:
      title: User Management API
      description: API for managing users, roles, and authentication across the platform
      version: 1.0.0
      termsOfService: https://example.com/terms
      contact:
        name: Platform Team
        email: platform@example.com
        url: https://example.com/platform

Hands on meaning:

- Who to contact when things break
- Who approves changes
- Who owns the contract

This prevents Slack chaos.

No guessing.
No blame games.

---

### 3.6 license info

Now add license information.

    info:
      title: User Management API
      description: API for managing users, roles, and authentication across the platform
      version: 1.0.0
      termsOfService: https://example.com/terms
      contact:
        name: Platform Team
        email: platform@example.com
        url: https://example.com/platform
      license:
        name: Apache 2.0
        url: https://www.apache.org/licenses/LICENSE-2.0.html

Hands on meaning:

- Defines usage rights
- Required for public APIs
- Important for legal clarity

Even internal APIs benefit from explicit licensing.

---

### 3.7 Why accurate metadata matters in teams

Now step back and look at your `info` object.

It now answers:

- What this API is
- Why it exists
- Who owns it
- How it can be used
- Which version clients rely on

In teams, this enables:

- Faster onboarding
- Clear ownership
- Safer changes
- Less miscommunication

Without accurate `info`:

- APIs feel anonymous
- Ownership is unclear
- Trust erodes

Metadata is not optional at scale.

---

### 4. Validate what you built

Open your file in Swagger UI or Swagger Editor.

You should see:

- Clear API name
- Rich description
- Contact details
- Legal info

All without writing a single endpoint.

That is how powerful the `info` object is.

---

### 5. Common beginner mistakes to avoid

Do NOT:

- Use vague titles
- Skip description
- Use build numbers as versions
- Leave contact empty
- Treat info as decoration

This section defines API identity.

