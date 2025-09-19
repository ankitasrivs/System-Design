# GraphQL vs REST vs SOAP

This document explains **GraphQL**, and compares it with **REST** and **SOAP** in terms of system design, use cases, and trade-offs.

---

## ❓ What is GraphQL?

* **GraphQL** is a **query language for APIs** and a **runtime for executing those queries**, developed by Facebook in 2012 and open-sourced in 2015.
* It provides a **flexible and efficient alternative** to REST by allowing clients to **ask for exactly the data they need** and nothing more.
* GraphQL APIs are organized in terms of **types** and **fields**, not endpoints.
* Instead of multiple endpoints, GraphQL typically exposes a single endpoint (`/graphql`).
* Uses a **strongly typed schema** to define the shape of available data.

---

## ⚡ GraphQL Features

* **Single Endpoint**: All requests go through one endpoint.
* **Declarative Data Fetching**: Clients specify exactly what they need.
* **Strongly Typed**: Schema defines data types and relationships.
* **Real-Time Support**: Subscriptions allow live data updates.
* **Introspection**: APIs are self-documenting.

### ✅ Advantages

* Avoids **over-fetching** and **under-fetching**.
* Efficient for mobile and low-bandwidth environments.
* Strong contracts with schema.
* Evolves easily without breaking clients.

### ❌ Disadvantages

* Complex server-side implementation.
* Harder caching than REST.
* Potential performance risks with deeply nested queries.
* Learning curve for teams used to REST.

---

## 📊 GraphQL Examples

### Example: Fetch User Data

#### GraphQL Query

```graphql
{
  user(id: 1) {
    id
    name
    email
  }
}
```

➡️ Returns only the requested fields.

#### REST Equivalent

```http
GET /users/1
```

```json
{
  "id": 1,
  "name": "Alice",
  "email": "alice@example.com",
  "address": "123 Street, City",
  "phone": "1234567890"
}
```

➡️ REST may return more fields than needed (**over-fetching**).

---

### Example: Nested Data

#### GraphQL Query

```graphql
{
  user(id: 1) {
    name
    posts(limit: 2) {
      title
      comments {
        text
      }
    }
  }
}
```

➡️ Single query retrieves user info, posts, and comments.

#### REST Equivalent

```http
GET /users/1
GET /users/1/posts?limit=2
GET /posts/10/comments
GET /posts/11/comments
```

➡️ Requires **multiple round-trips** (**under-fetching**).

---

### Example: Mutation (Update User)

#### GraphQL Mutation

```graphql
mutation {
  updateUser(id: 1, input: { email: "new@example.com" }) {
    id
    name
    email
  }
}
```

➡️ Updates and returns requested fields.

#### REST Equivalent

```http
PATCH /users/1
{
  "email": "new@example.com"
}
```

➡️ Updates but typically returns full or no resource.

---

## 🌍 REST (Representational State Transfer)

* **Architectural style** using standard HTTP methods (GET, POST, PUT, PATCH, DELETE).
* Each resource is exposed via a **separate endpoint**.
* Data usually in **JSON**.

### ✅ Advantages

* Simple, widely adopted.
* Easy caching via HTTP.
* Flexible data formats.
* Works seamlessly with browsers and mobile clients.

### ❌ Disadvantages

* **Over-fetching** → Client receives more data than needed.
* **Under-fetching** → Client needs multiple requests to get all data.
* No strict schema (unless using OpenAPI/Swagger).

---

## 🧩 SOAP (Simple Object Access Protocol)

* **Protocol** with strict standards.
* Uses only **XML**.
* Designed for **enterprise apps** needing **high security** and **transaction reliability**.

### ✅ Advantages

* Built-in **WS-Security**.
* Strong contracts via **WSDL**.
* Supports **ACID transactions**.
* Transport-agnostic (HTTP, SMTP, TCP, etc.).

### ❌ Disadvantages

* Heavy XML payloads.
* Slower than REST/GraphQL.
* Complex to implement.

---

## 🆚 SOAP vs REST vs GraphQL (Comparison)

| Feature         | SOAP                               | REST                             | GraphQL                              |
| --------------- | ---------------------------------- | -------------------------------- | ------------------------------------ |
| **Type**        | Protocol                           | Architectural style              | Query Language + Runtime             |
| **Data Format** | XML only                           | JSON (commonly), XML, YAML, etc. | JSON                                 |
| **Transport**   | HTTP, SMTP, TCP, more              | HTTP only                        | HTTP (usually POST to /graphql)      |
| **Security**    | WS-Security (built-in)             | HTTPS + OAuth/JWT (external)     | HTTPS + OAuth/JWT (external)         |
| **Contract**    | WSDL                               | Optional (OpenAPI/Swagger)       | Strong schema (SDL)                  |
| **Performance** | Slower (heavy XML)                 | Medium (may over/under-fetch)    | Faster (exact queries, efficient)    |
| **Ease of Use** | Complex (strict)                   | Simple, widely adopted           | Powerful but more complex            |
| **Use Case**    | Enterprise apps (finance, telecom) | Web & mobile apps, microservices | Modern apps needing flexible queries |

---

## 🎯 When to Use

* **SOAP**: Enterprise, high-security, transactional systems.
* **REST**: General-purpose web/mobile APIs, easy to cache.
* **GraphQL**: Data-heavy applications, mobile apps, microservices with complex relationships.

---
