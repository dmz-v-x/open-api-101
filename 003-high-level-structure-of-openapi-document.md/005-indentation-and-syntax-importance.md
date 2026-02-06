## Indentation & Syntax Importance

### 1. Goal of this hands on step

You are going to **break and fix a real OpenAPI file** to understand why YAML indentation and syntax rules are critical.

No theory first.  
You will **see tools fail**, then **see them work again**.

By the end, indentation will no longer feel optional.

---

### 2. Critical mindset before touching YAML

YAML is not like JSON.

In YAML:

- Indentation defines structure  
- Spaces replace brackets  
- One wrong space can break everything  

OpenAPI is strict.  
YAML is strict.  

That combination demands discipline.

---

### 3. Start with a known working file

Make sure your `openapi.yaml` looks exactly like this:

    openapi: 3.0.3

    info:
      title: YAML Rules Demo API
      version: 1.0.0

    paths:
      /health:
        get:
          summary: Health check
          responses:
            '200':
              description: API is healthy

This file is valid.

Open it in Swagger Editor or Swagger UI and confirm there are **no errors**.

---

### 4. Rule one indentation equals hierarchy

Now look closely at this part:

    paths:
      /health:
        get:

Each indentation level means:

- `paths` is the parent  
- `/health` is inside `paths`  
- `get` is inside `/health`  

YAML uses **spaces to express nesting**.

There are no braces to save you.

---

### 5. Break it on purpose wrong indentation

Now intentionally break it.

Change this:

    paths:
      /health:

To this:

    paths:
    /health:

Save the file and reload it in the editor.

What happens:

- Immediate syntax error  
- Tool cannot parse the file  
- Nothing renders  

You just removed the hierarchy.

---

### 6. Fix it restore correct indentation

Fix it back to:

    paths:
      /health:

Reload the file.

Result:

- Error disappears  
- Spec works again  

This proves indentation is not cosmetic.
It is structural.

---

### 7. Rule two consistent spacing only spaces

YAML **does not allow tabs**.

Now try this experiment:

- Replace indentation spaces with a tab character  
- Save the file  
- Reload it  

Result:

- Parser error  
- Invalid YAML  

Even though it looks aligned, YAML rejects it.

Rule to lock in:

Only spaces.  
Never tabs.

---

### 8. Rule three alignment matters across siblings

Look at this correct structure:

    get:
      summary: Health check
      responses:

Now break it subtly:

    get:
       summary: Health check
      responses:

Notice the extra space before `summary`.

Reload the file.

Result:

- YAML error or unexpected structure  
- Tools get confused  

Why:

- `summary` is no longer aligned with `responses`  
- YAML thinks they belong to different levels  

One extra space changes meaning.

---

### 9. Rule four lists must align perfectly

Now add a servers block:

    servers:
      - url: https://api.example.com

This is correct.

Now break it:

    servers:
        - url: https://api.example.com

Or this:

    servers:
      -url: https://api.example.com

Reload.

Result:

- Parsing error  

Lists require:

- Dash and value spacing  
- Correct alignment under parent  

YAML lists are very strict.

---

### 10. Rule five strings and quotes matter

Now look at this response code:

    '200':
      description: API is healthy

Quotes are required because:

- YAML treats numbers differently  
- OpenAPI requires response codes as strings  

Now remove quotes:

    200:
      description: API is healthy

Reload.

Result:

- OpenAPI validation error  

YAML accepts it.
OpenAPI does not.

Syntax and spec rules both apply.

---

### 11. YAML valid does not mean OpenAPI valid

Important lesson:

- A file can be valid YAML  
- And still be invalid OpenAPI  

YAML checks structure.
OpenAPI checks meaning.

Both must pass.

---

### 12. How tools actually fail in practice

When indentation or syntax is wrong:

- Tools fail early  
- Errors point to lines, not intent  
- One mistake hides others  

This is why small YAML errors feel painful.

They stop everything downstream.

---

### 13. Defensive habits you should build now

Always do this:

- Indent using editor auto-format  
- Enable YAML linting  
- Validate after every change  
- Change one thing at a time  

Never hand-wave indentation.


