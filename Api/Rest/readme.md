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


| HTTP Method | Action | Example Endpoint | Description |
|-------------|--------|------------------|-------------|
| **GET** | Read | `/users` | Fetch list of users |
| **GET** | Read | `/users/123` | Fetch user by ID |
| **POST** | Create | `/users` | Create a new user |
| **PUT** | Update | `/users/123` | Update existing user (replace) |
| **PATCH** | Update | `/users/123` | Update partially (e.g., only email) |
| **DELETE** | Delete | `/users/123` | Remove user |


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
