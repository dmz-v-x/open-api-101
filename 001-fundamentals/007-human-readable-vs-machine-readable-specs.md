## Human Readable vs Machine Readable Specs

### 1. Start with a simple question

When you document an API, who is that documentation for?

Humans  
Machines  
Or both  

This question is the core reason OpenAPI exists.

---

### 2. What is a human readable spec

A human readable spec is documentation written for people.

Common examples include:

- Markdown documentation  
- README files  
- Wiki pages  
- Notion or Confluence docs  
- Blog posts  

These are meant to be:

- Read  
- Understood  
- Interpreted by humans  

They rely on natural language.

---

### 3. Pros of human readable specs

Human readable specs are:

- Easy to write  
- Easy to explain concepts with  
- Friendly for beginners  

They work very well for:

- Tutorials  
- Conceptual explanations  
- High level overviews  

They are excellent teaching tools.

---

### 4. Problems with human readable specs important

Human readable specs have serious limitations.

They are:

- Ambiguous  
- Interpreted differently by different people  
- Easy to forget to update  
- Impossible to validate automatically  

Example problem:

“This field is usually present”

What does usually mean to a machine?

Nothing.

Natural language is vague by default.

---

### 5. What is a machine readable spec

A machine readable spec is a description written in a strict and structured format that computers can understand.

Its characteristics are:

- No ambiguity  
- Strict rules  
- Precise structure  
- Parsable by tools  

OpenAPI is exactly this kind of specification.

---

### 6. What machines can do with a machine readable spec

Because the spec is precise, machines can:

- Generate documentation automatically  
- Generate client SDKs  
- Generate server stubs  
- Validate requests and responses  
- Detect breaking changes  
- Power mock servers  
- Power automated testing tools  

Humans alone cannot do these things reliably.

---

### 7. Why humans alone are not enough

Humans:

- Forget to update documentation  
- Miss edge cases  
- Make assumptions  
- Interpret language differently  

Machines:

- Enforce rules strictly  
- Never forget  
- Never guess  
- Apply rules consistently  

OpenAPI shifts API reliability from trusting humans to trusting automation.

---

### 8. OpenAPI is machine readable first

This is an important mindset shift.

OpenAPI is written for machines first, humans second.

In practice, humans usually interact with:

- Generated documentation  
- Generated client libraries  
- Generated mock servers  

Not with the raw OpenAPI file itself.

---

### 9. How OpenAPI bridges both worlds

OpenAPI acts as:

- A machine readable source of truth  

From that source, you generate:

- Human readable documentation  
- Client SDKs  
- Testing and validation tools  

So instead of:

Writing documentation by hand  

You:

Write the specification  
Generate everything else  

---

### 10. Very concrete comparison

Human readable only:

- This endpoint returns a user object  
- Sometimes returns an error  
- Field may be null  

Machine readable:

- Field types are explicitly defined  
- Required versus optional fields are explicit  
- Exact error responses are defined  

There is no guessing.

---

### 11. Final takeaway

Human readable specs explain APIs to people.

Machine readable specs allow tools to enforce, validate, and automate APIs.

OpenAPI exists to make APIs precise enough for machines while still being useful for humans.
