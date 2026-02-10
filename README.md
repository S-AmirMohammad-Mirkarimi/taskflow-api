<div align="center">

# ✅ TaskFlow API — Task Management REST API

A minimal **RESTful Task Manager API** built with **Node.js**, **Express**, and **MongoDB (Mongoose)**.  
**Learning project** focused on backend fundamentals: **REST APIs**, **JWT authentication**, and database operations.  
Includes **user/task CRUD**, **filtering/pagination/sorting**, **password hashing**, and **avatar upload** with basic image processing.

<br/>

![Node.js](https://img.shields.io/badge/Node.js-Backend-339933?logo=node.js&logoColor=white)
![Express](https://img.shields.io/badge/Express-API-000000?logo=express&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-Database-47A248?logo=mongodb&logoColor=white)
![Mongoose](https://img.shields.io/badge/Mongoose-ODM-880000)
![JWT](https://img.shields.io/badge/JWT-Auth-000000?logo=jsonwebtokens&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6-F7DF1E?logo=javascript&logoColor=000)
![Uploads](https://img.shields.io/badge/Uploads-Multer-blue)
![Images](https://img.shields.io/badge/Image%20Processing-Sharp-6b7280)

</div>

---

## 📌 Table of Contents
- [✨ Features](#-features)
- [🧰 Tech Stack](#-tech-stack)
- [📁 Project Structure](#-project-structure)
- [🚀 Getting Started](#-getting-started)
- [🔐 Environment Variables](#-environment-variables)
- [🔑 Authentication](#-authentication)
- [📡 API Reference](#-api-reference)
  - [User Endpoints](#user-endpoints)
  - [Task Endpoints](#task-endpoints)
  - [Query Params for Tasks](#query-params-for-tasks)
- [🧪 Examples (cURL)](#-examples-curl)
- [🧠 Data Models](#-data-models)
- [👤 Author](#-author)

---

## What I practiced
- Designing REST endpoints and working with query parameters (filter/sort/pagination)
- Authentication and password hashing (JWT, bcrypt)
- Validation, error handling, and writing clear API documentation

---

## ✨ Features

### ✅ Auth & Users
- 🔐 JWT authentication (token stored per device/session)
- 🔒 Password hashing with `bcryptjs`
- 👤 Profile endpoints (get/update/delete)
- 🖼️ Avatar upload / fetch / delete
  - Accepts `jpg|jpeg|png`
  - Max size: **1MB**
  - Auto-resize to **250×250** and convert to **PNG**

### ✅ Tasks
- ✅ Create / Read / Update / Delete tasks (user-scoped)
- 🔎 Filter by completion status (`completed=true|false`)
- 📄 Pagination (`limit` & `skip`)
- ↕️ Sorting (`sortBy=createdAt:desc`)

---

## 🧰 Tech Stack

| Category | Technology |
|---|---|
| Runtime | Node.js |
| Framework | Express.js |
| Database | MongoDB |
| ODM | Mongoose |
| Auth | JSON Web Token (JWT) |
| Security | bcryptjs |
| Validation | validator |
| File Upload | multer |
| Image Processing | sharp |
| Env Config | dotenv |

---

## 📁 Project Structure

```txt
taskflow-api/
├─ src/
│  ├─ controllers/
│  │  ├─ task.js
│  │  └─ user.js
│  ├─ middleware/
│  │  └─ auth.js
│  ├─ models/
│  │  ├─ task.js
│  │  └─ user.js
│  ├─ routers/
│  │  ├─ task.js
│  │  └─ user.js
│  └─ index.js
├─ package.json
└─ package-lock.json
````

---

## 🚀 Getting Started

### ✅ Prerequisites

* Node.js installed
* MongoDB running locally **or** a MongoDB Atlas connection string

### 📥 Installation

```bash
git clone https://github.com/S-AmirMohammad-Mirkarimi/taskflow-api.git
cd taskflow-api
npm install
```

### 🧪 Development (auto-restart)

```bash
npm run dev
```

### ▶️ Run

```bash
npm start
```

---

## 🔐 Environment Variables

This project uses `dotenv` and expects a **.env file in the project root**.

Create a file named `.env`:

```env
PORT=3000
MONGODB_URL=mongodb://127.0.0.1:27017/taskflow-api
JWT_SECRET=yourStrongJwtSecret
```

> ✅ Important: never commit `.env` (add it to `.gitignore`).

---

## 🔑 Authentication

Protected routes require this header:

```http
Authorization: Bearer <JWT_TOKEN>
```

> Note: The server expects the string **`Bearer `** (with a space).

---

## 📡 API Reference

### User Endpoints

| Method | Endpoint            | Description                             | Auth |
| -----: | ------------------- | --------------------------------------- | :--: |
|   POST | `/users`            | Create user (returns `{ user, token }`) |   ❌  |
|   POST | `/users/login`      | Login (returns `{ user, token }`)       |   ❌  |
|   POST | `/users/logout`     | Logout current session                  |   ✅  |
|   POST | `/users/logoutAll`  | Logout from all sessions                |   ✅  |
|    GET | `/users/me`         | Get your profile                        |   ✅  |
|  PATCH | `/users/me`         | Update profile                          |   ✅  |
| DELETE | `/users/me`         | Delete account                          |   ✅  |
|   POST | `/users/me/avatar`  | Upload avatar (multipart form-data)     |   ✅  |
|    GET | `/users/:id/avatar` | Get user avatar (png)                   |   ❌  |
| DELETE | `/users/me/avatar`  | Delete your avatar                      |   ✅  |

**Update allowed fields (`PATCH /users/me`):**

* `name`, `age`, `email`, `password`

---

### Task Endpoints

| Method | Endpoint     | Description                      | Auth |
| -----: | ------------ | -------------------------------- | :--: |
|   POST | `/tasks`     | Create task                      |   ✅  |
|    GET | `/tasks`     | List tasks (supports filters)    |   ✅  |
|    GET | `/tasks/:id` | Get task by id (only your tasks) |   ✅  |
|  PATCH | `/tasks/:id` | Update task by id                |   ✅  |
| DELETE | `/tasks/:id` | Delete task by id                |   ✅  |

**Update allowed fields (`PATCH /tasks/:id`):**

* `description`, `completed`

---

### Query Params for Tasks

`GET /tasks` supports:

| Query       | Example                  | Description               |
| ----------- | ------------------------ | ------------------------- |
| `completed` | `?completed=true`        | Filter by completion      |
| `limit`     | `?limit=10`              | Page size                 |
| `skip`      | `?skip=0`                | Offset                    |
| `sortBy`    | `?sortBy=createdAt:desc` | Sort by field & direction |

---

## 🧪 Examples (cURL)

### 1) Create User

```bash
curl -X POST http://localhost:3000/users \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Pedram",
    "email": "pedram@example.com",
    "password": "MyPass123!",
    "age": 25
  }'
```

### 2) Login

```bash
curl -X POST http://localhost:3000/users/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "pedram@example.com",
    "password": "MyPass123!"
  }'
```

### 3) Create Task (Authenticated)

```bash
curl -X POST http://localhost:3000/tasks \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <JWT_TOKEN>" \
  -d '{
    "description": "Finish the README",
    "completed": false
  }'
```

### 4) List Tasks (filter + pagination + sort)

```bash
curl "http://localhost:3000/tasks?completed=false&limit=10&skip=0&sortBy=createdAt:desc" \
  -H "Authorization: Bearer <JWT_TOKEN>"
```

### 5) Upload Avatar (max 1MB, jpg/jpeg/png)

```bash
curl -X POST http://localhost:3000/users/me/avatar \
  -H "Authorization: Bearer <JWT_TOKEN>" \
  -F "avatar=@/path/to/avatar.jpg"
```

### 6) Get Avatar

```bash
curl http://localhost:3000/users/<USER_ID>/avatar --output avatar.png
```

---

## 🧠 Data Models

### User

* `name` (required, trimmed)
* `email` (unique, validated, lowercased)
* `password` (min length 7, cannot contain "password")
* `age` (must be positive)
* `tokens[]` (JWT tokens per session)
* `avatar` (stored as Buffer)
* timestamps: `createdAt`, `updatedAt`

✅ Sensitive fields are hidden automatically in JSON responses:

* `password`, `tokens`, `avatar`

### Task

* `description` (required, trimmed)
* `completed` (default: false)
* `owner` (User ObjectId ref)
* timestamps: `createdAt`, `updatedAt`

✅ Relationship:

* A user has many tasks via a virtual field (`user.tasks`)
* When a user is deleted, their tasks are deleted as well.

---

## 👤 Author

**S. AmirMohammad Mirkarimi**
GitHub: [S-AmirMohammad-Mirkarimi](https://github.com/S-AmirMohammad-Mirkarimi)
