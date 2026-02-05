## What is a Contract

### 1. Start with a human idea no tech

Imagine two people working together.

One person asks for something.  
The other person promises how they will respond.

For this to work smoothly, both people must agree on a few things:

- What can be asked  
- How it should be asked  
- What will be returned  

This shared agreement is called a **contract**.

Without this agreement, misunderstandings happen very quickly.

---

### 2. What is a contract in plain English

A contract is a clear agreement that defines what each side expects and promises.

At its core:

- One side promises a behavior  
- The other side relies on that promise  

Nothing more. Nothing less.

It is about expectations and trust.

---

### 3. Contract is not implementation important

A contract does not describe:

- How something is implemented  
- How the code works internally  
- What database is used  
- What programming language is chosen  

A contract only says:

If you do X, I promise to do Y.

How Y is achieved internally is irrelevant to the contract.

---

### 4. Contract in software systems

In software, contracts appear everywhere, especially in APIs.

There are always two sides:

- Client  
  - Frontend  
  - Mobile app  
  - Another backend service  

- Server  
  - Backend API  

Both sides must agree on:

- Which endpoints exist  
- How requests should be sent  
- What responses will look like  
- How errors are returned  
- Which status codes are used  

This agreement is called the **API contract**.

---

### 5. What an API contract includes high level

An API contract typically defines only the following:

- What URLs exist  
- What HTTP methods are allowed  
- What request data looks like  
- What response data looks like  
- What errors are possible  

And that is it.

A good contract includes nothing extra and hides nothing important.

---

### 6. Why contracts are about trust

Contracts exist because systems do not share the same codebase or ownership.

A client trusts that:

- The server will behave exactly as documented  

A server trusts that:

- The client will follow the documented rules  

When either side breaks the contract:

- Things fail  
- Bugs appear  
- Debugging becomes painful  
- Teams start blaming each other 😄  

Strong contracts reduce friction between teams.

---

### 7. Very simple REST example

Imagine the contract says:

- Endpoint: GET /users/{id}  
- Response shape:  
  - id  
  - name  
  - email  

The client relies on:

- The URL existing  
- The fields being present  
- The data types being correct  

The server promises:

- To always respond in this exact shape  

As long as both sides honor this promise, everything works.

---

### 8. What happens when contract changes

Problems start when the server changes things without updating the contract.

Examples:

- Renaming fields  
- Changing field types  
- Changing endpoint behavior  

If the contract is not updated:

- Clients break  
- Production bugs appear  
- Trust is lost  

That is why contracts must be:

- Stable  
- Explicit  
- Versioned when needed  

---

### 9. Contract is a boundary important

The contract is a strict boundary between client and server.

- The client should only depend on the contract  
- The server should only expose the contract  

Inside the server:

- Code can change  
- Databases can change  
- Architecture can change  

As long as the contract stays the same, clients remain unaffected.

---

### 10. Real world analogy very clear

Think of a restaurant.

- Menu is the contract  
- Kitchen is the server  
- Customer is the client  

The kitchen can:

- Change chefs  
- Change tools  
- Improve recipes  

As long as the menu stays the same, customers are happy.

The customer does not care how the dish is cooked internally.

---

### 11. Common beginner misunderstandings

Incorrect belief: Contract is the code  

No.

- Code changes frequently  
- Contracts should be stable  

Incorrect belief: We will figure it out as we go  

That usually leads to:

- Broken clients  
- Confusing integrations  
- Chaos  

Clear contracts upfront prevent long term pain.

---

### 12. Final definition

A contract is an agreed upon definition of how a client and a server communicate.

It defines expectations, builds trust, and creates a clear boundary between systems.
