## Contract vs API

### 1. Is contract and API the same thing

No.

They are different concepts, but they are closely connected.

An API and a contract are related, but they are not interchangeable.

---

### 2. Think in roles very simple

Think in terms of roles:

- API is what exists and runs  
- Contract is what is promised  

The contract describes the API, but it is not the API itself.

One is behavior.  
The other is the promise of that behavior.

---

### 3. What an API is recap

An API is the actual interface exposed by a server.

It includes:

- Real endpoints  
- Real logic  
- Real responses  

It is code plus behavior.

Example:

- A server running at api.example.com  
- An endpoint /users/123  
- Code executes and returns data  

That running system is the API.

---

### 4. What a contract is recap

A contract is not code.

A contract is:

- A description  
- A promise  
- An agreement  

It says:

“This API behaves like this.”

A contract:

- Does not run  
- Does not execute  
- Does not return data by itself  

It only defines expectations.

---

### 5. Key difference in one table

| Concept               | API        | Contract              |
|----------------------|------------|-----------------------|
| Is it running code   | Yes        | No                    |
| Is it a description  | No         | Yes                   |
| Can clients call it  | Yes        | No                    |
| Can it break clients | Yes        | No                    |
| Can it be documented | Yes        | That documentation is the contract |

---

### 6. Very concrete example

API real behavior:

GET /users/123  
Returns:  
{ "id": 123, "name": "Alice" }

This is the API doing real work.

---

Contract agreement:

GET /users/{id}  
Response:  
- id: number  
- name: string  

This is the promise of how the API should behave.

---

### 7. What happens if they do not match

Contract says the response will be:

{ "id": 123, "name": "Alice" }

But the API actually returns:

{ "userId": 123, "fullName": "Alice" }

Result:

- The API still exists  
- The server is still running  
- But the contract is broken  

Clients crash because they trusted the promise.

This situation is called a contract violation.

---

### 8. Relationship between API and contract important

A simple way to remember:

- API is the implementation  
- Contract is the specification  

The API must always follow the contract.

If implementation changes but the contract stays the same, clients are safe.

---

### 9. Where OpenAPI fits

OpenAPI is a contract format.

It is:

- A way to write down the agreement  
- A machine readable description of the API  
- A shared source of truth  

OpenAPI is not the API itself.

It describes how the API behaves.

---

### 10. Final clarity

An API is the actual interface that runs and handles requests.

A contract is the documented agreement that describes how that API behaves.
