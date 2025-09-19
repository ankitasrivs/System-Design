# 🌐 REST API in System Design (Complete Guide)

## 1. What is REST?
- **REST (Representational State Transfer)** is an **architectural style** for designing APIs.
- It’s based on **stateless client-server communication** over **HTTP**.
- Each request contains all info needed (stateless), and the server responds with a representation of a resource (JSON, XML, etc.).

---

## 2. REST API Principles
These are **core interview talking points**:

1. **Stateless** – each request is independent.  
2. **Client-Server Separation** – frontend (iOS app) and backend (server) evolve independently.  
3. **Uniform Interface** – standard endpoints, predictable URIs (`/users`, `/orders/123`).  
4. **Resource-Based** – everything is a resource (user, post, image).  
5. **Cacheable** – responses can be cached (important for scalability).  
6. **Layered System** – can introduce CDNs, proxies, load balancers without affecting clients.  

---

## 3. REST API Endpoints & Methods
**Resources are nouns, HTTP methods define actions:**

| HTTP Method | Action  | Example Endpoint | Description |
|-------------|---------|------------------|-------------|
| **GET**     | Read    | `/users`         | Fetch list of users |
| **GET**     | Read    | `/users/123`     | Fetch user by ID |
| **POST**    | Create  | `/users`         | Create a new user |
| **PUT**     | Update  | `/users/123`     | Update existing user (replace) |
| **PATCH**   | Update  | `/users/123`     | Update partially (optimized) |
| **DELETE**  | Delete  | `/users/123`     | Remove user |

---

## 4. REST API Request Structure
### Example Request
```http
POST /users HTTP/1.1
Host: api.example.com
Content-Type: application/json
Authorization: Bearer <token>

{
  "name": "Ankita",
  "email": "ankita@example.com"
}
```
## 🆚 PUT vs PATCH


### **PUT**
- Replaces the **entire resource** with the provided payload.
- Example:
```http
PUT /users/123
{
"name": "Alice",
"email": "alice@example.com"
}
```
➡️ If only `email` changes, the full object still must be sent.


### **PATCH**
- Updates only the **fields that changed**.
- Example:
```http
PATCH /users/123
{
"email": "new@example.com"
}
```
➡️ Only `email` is updated, other fields remain unchanged.


### **Comparison**
| Feature | PUT (Replace) | PATCH (Partial Update) |
|-----------------|---------------|-------------------------|
| Payload Size | Larger (full object) | Smaller (only changes) |
| Performance | More processing | Optimized (less work) |
| Risk | Overwrites fields if missing | Might cause partial update conflicts |
| Usage | Full replace | Partial update |


---


## 🔧 PATCH Optimization
- **Reduced Payload Size** → Only changed fields are sent.
- **Reduced Server Processing** → Only partial updates applied.
- **Network Bandwidth Savings** → Critical for mobile (e.g., iOS apps).


### How PATCH Works
- **JSON Merge Patch (RFC 7386)**: Merge given fields into resource.
- **JSON Patch (RFC 6902)**: Express changes as operations (add, remove, replace).


#### JSON Merge Patch Example
```http
PATCH /users/123
Content-Type: application/merge-patch+json
{
"email": "new@example.com"
}
```


#### JSON Patch Example
```http
PATCH /users/123
Content-Type: application/json-patch+json
[
{ "op": "replace", "path": "/email", "value": "new@example.com" }
]
```


---
### Key Parts
- **Endpoint/URL** – `/users`
- **HTTP Method** – POST
- **Headers** – metadata (`Authorization`, `Content-Type`)
- **Body** – request payload (JSON)

---

## 5. REST API Response Structure
### Example Response
```http
201 Created
Content-Type: application/json

{
  "id": 123,
  "name": "Ankita",
  "email": "ankita@example.com",
  "createdAt": "2025-09-19T10:00:00Z"
}
```

### Key Parts
- **Status Code** – 201 (created successfully)
- **Headers** – `Content-Type: application/json`
- **Body** – JSON data

---

## 6. HTTP Status Codes (Important for Interviews)
- **200 OK** – Success (GET, PUT, PATCH)
- **201 Created** – Resource created (POST)
- **204 No Content** – Success, no response body (DELETE)
- **400 Bad Request** – Invalid input
- **401 Unauthorized** – Invalid/missing authentication
- **403 Forbidden** – Authenticated but not allowed
- **404 Not Found** – Resource doesn’t exist
- **500 Internal Server Error** – Server crash/bug

---

## 7. Authentication in REST APIs
1. **API Keys** – Simple, but less secure.
2. **OAuth 2.0 / JWT (JSON Web Tokens)** – Modern, stateless authentication.
   - Client sends `Authorization: Bearer <token>` in header.
   - Server validates token without keeping session state.

---

## 8. Versioning REST APIs
- `https://api.example.com/v1/users`
- `https://api.example.com/v2/users`

**Why?** To allow clients to keep working when server APIs evolve.

---

## 9. Caching Strategies
- **Client-Side Cache** – `URLCache`, `NSCache` in iOS.
- **Server-Side Cache** – Redis, Memcached.
- **HTTP Cache Headers**:
  - `Cache-Control: max-age=3600`
  - `ETag` – validate if resource has changed.

---

## 10. Pagination & Filtering
Large datasets shouldn’t be fetched at once.

- **Offset/Limit**: `/users?offset=0&limit=50`
- **Cursor-Based**: `/users?after=cursor123`
- **Filtering**: `/users?country=india`
- **Sorting**: `/users?sort=createdAt_desc`

---

## 11. Rate Limiting & Throttling
To protect servers from abuse:
- **Limit** requests (e.g., 100 requests/minute).
- Use headers like `X-RateLimit-Limit`, `X-RateLimit-Remaining`.

---

## 12. PATCH Optimization

### Difference Between PUT and PATCH
- **PUT** → Replaces the *entire resource*. Even if one field changes, the whole object is sent.
- **PATCH** → Updates only the fields that changed.

### Why PATCH is Optimized
- **Reduced Payload Size** → Send only changed fields.
- **Reduced Server Processing** → Apply minimal changes instead of rewriting.
- **Bandwidth Savings** → Smaller requests = faster, cheaper (especially on mobile).

### JSON Merge Patch Example
```http
PATCH /users/123
Content-Type: application/merge-patch+json

{
  "email": "new@example.com"
}
```

### JSON Patch Example
```http
PATCH /users/123
Content-Type: application/json-patch+json

[
  { "op": "replace", "path": "/email", "value": "new@example.com" }
]
```

👉 In interviews: Emphasize that PATCH is **more efficient** for partial updates and ideal for **mobile clients** like iOS apps.

---

## 13. REST API Best Practices
- Use **plural nouns** (`/users`, `/posts`).
- **Use nouns, not verbs** (`/users/123` not `/getUser`).
- Support **filtering, sorting, pagination**.
- Return **proper status codes**.
- Make APIs **stateless & idempotent**.
- Provide **clear error messages**.
- Secure with **HTTPS + authentication**.

---

## 14. REST vs Alternatives
- **REST**: Simple, widely adopted, cache-friendly.
- **GraphQL**: Flexible queries, fewer round trips.
- **gRPC**: High performance, strongly typed, binary protocol.
- **WebSockets**: For real-time two-way communication.

---

## ✅ Interview Tip
When asked about REST:
1. **Define** it clearly.
2. **Explain principles** (stateless, resource-based).
3. **Show request/response with status codes.**
4. **Discuss caching, pagination, authentication, error handling.**
5. **Compare with GraphQL/WebSockets when relevant.**
