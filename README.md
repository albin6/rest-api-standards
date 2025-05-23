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

## 📌 Additional Production-Level Practices

### ✅ Rate Limiting and Throttling

Prevent abuse and limit repeated requests:

```js
const rateLimit = require("express-rate-limit");
const limiter = rateLimit({ windowMs: 15 * 60 * 1000, max: 100 });
app.use(limiter);
```

### ✅ Input Sanitization to Prevent Injection

Use libraries like `express-validator`, `xss-clean`:

```bash
npm install express-validator xss-clean
```

```js
const xss = require("xss-clean");
app.use(xss());
```

### ✅ Use HTTPS

Always use HTTPS in production to encrypt data in transit.

### ✅ Centralized Error Handling

Create a global error handler to catch and respond to errors consistently:

```js
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ success: false, error: { message: "Server error" } });
});
```

### ✅ Graceful Shutdown

Close connections properly before shutting down the server:

```js
process.on("SIGTERM", () => {
  server.close(() => {
    console.log("Process terminated");
  });
});
```

### ✅ Logging

Use tools like `winston`, `morgan`, or integrate with cloud log systems:

```bash
npm install winston
```

### ✅ Monitor and Alert

Use services like Prometheus, Grafana, or DataDog to monitor performance and uptime.

### ✅ Data Validation Schemas

Use `Joi`, `Zod`, or `Yup` to define clear data validation rules for APIs.

---
