# RESTful API Standards & Best Practices

This document provides a comprehensive guide to RESTful API best practices to help maintain clean, secure, and scalable APIs.

---

## 📌 HTTP Methods and Usage

| Method   | Purpose                      | Example             |
| -------- | ---------------------------- | ------------------- |
| `GET`    | Retrieve data                | `GET /users`        |
| `POST`   | Create a new resource        | `POST /users`       |
| `PUT`    | Replace an existing resource | `PUT /users/123`    |
| `PATCH`  | Update part of a resource    | `PATCH /users/123`  |
| `DELETE` | Delete a resource            | `DELETE /users/123` |

---

## 📌 Naming Conventions

* Use **plural nouns** for resource names.
* Avoid verbs in endpoint paths.

**✅ Good:**

```
GET /users
GET /users/:id
POST /users
```

**🚫 Bad:**

```
GET /getUser
POST /createUser
```

---

## 📌 HTTP Status Codes

| Code | Meaning               | Use Case                          |
| ---- | --------------------- | --------------------------------- |
| 200  | OK                    | Successful `GET`, `PUT`, `DELETE` |
| 201  | Created               | After successful `POST`           |
| 204  | No Content            | Success, no data to return        |
| 400  | Bad Request           | Invalid input                     |
| 401  | Unauthorized          | Missing/invalid auth              |
| 403  | Forbidden             | Authenticated, but access denied  |
| 404  | Not Found             | Resource not found                |
| 500  | Internal Server Error | Server error                      |

---

## 📌 Versioning Your API

Use versioning to manage changes without breaking existing clients.

**✅ Example:**

```
GET /api/v1/users
```

---

## 📌 Filtering, Sorting, Pagination

Use query parameters:

```
GET /products?category=phones&sort=price&limit=10&page=2
```

---

## 📌 Use Middleware for Common Tasks

* Authentication (e.g., `verifyToken`)
* Logging (`morgan`)
* Input validation (`express-validator`, `Joi`, `Zod`)
* Error handling

---

## 📌 Request/Response Best Practices

### Request:

* Validate and sanitize input.

### Response:

```
{
  "success": true,
  "data": { ... },
  "message": "Resource fetched successfully"
}
```

---

## 📌 Use `id` or `slug` in URLs

**✅:**

```
GET /posts/iphone-15-review
GET /users/609d5d52e93b4c30a4e9e18d
```

---

## 📌 Authentication & Authorization

* Use JWT, OAuth, or API Keys.
* Send tokens in headers:

```
Authorization: Bearer <token>
```

---

## 📌 Consistent Error Format

```
{
  "success": false,
  "error": {
    "code": 400,
    "message": "Invalid email format"
  }
}
```

---

## 📌 Avoid Deep Nesting in Routes

**✅:**

```
GET /users/123/orders
```

**🚫:**

```
GET /users/123/orders/456/products/789/items
```

---

## 📌 CORS Support

Use `cors` middleware for cross-origin requests:

```js
const cors = require("cors");
app.use(cors());
```

---

## 📌 Security Middleware

Install and use:

```bash
npm install helmet express-rate-limit
```

```js
const helmet = require("helmet");
const rateLimit = require("express-rate-limit");

app.use(helmet());
app.use(rateLimit({ windowMs: 1 * 60 * 1000, max: 100 }));
```

---

## 📌 Documentation

* Swagger (OpenAPI)
* Postman Collections
* README with endpoint descriptions

---

## 📌 Use Environment Variables

Avoid hardcoding secrets:

```
PORT=3000
MONGO_URI=mongodb://...
JWT_SECRET=supersecret
```

Use them in code:

```js
require('dotenv').config();
const port = process.env.PORT;
```

---

