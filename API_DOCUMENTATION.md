# API Documentation

Base URL: `http://localhost:5000/api`

## Authentication Endpoints

### 1. User Signup

**Endpoint:** `POST /auth/signup`

**Description:** Register a new user account

**Request Body:**
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123"
}
```

**Response:** (201 Created)
```json
{
  "success": true,
  "data": {
    "_id": "user_id",
    "name": "John Doe",
    "email": "john@example.com",
    "token": "jwt_token_here"
  }
}
```

**Validation:**
- `name`: Required, non-empty string
- `email`: Required, valid email format
- `password`: Required, minimum 6 characters

---

### 2. User Login

**Endpoint:** `POST /auth/login`

**Description:** Authenticate user and receive JWT token

**Request Body:**
```json
{
  "email": "john@example.com",
  "password": "password123"
}
```

**Response:** (200 OK)
```json
{
  "success": true,
  "data": {
    "_id": "user_id",
    "name": "John Doe",
    "email": "john@example.com",
    "token": "jwt_token_here"
  }
}
```

---

### 3. Get User Profile

**Endpoint:** `GET /auth/profile`

**Description:** Get current user's profile information

**Headers:**
```
Authorization: Bearer {jwt_token}
```

**Response:** (200 OK)
```json
{
  "success": true,
  "data": {
    "_id": "user_id",
    "name": "John Doe",
    "email": "john@example.com",
    "bio": "Software developer",
    "avatar": "",
    "createdAt": "2024-01-01T00:00:00.000Z"
  }
}
```

---

### 4. Update User Profile

**Endpoint:** `PUT /auth/profile`

**Description:** Update user profile information

**Headers:**
```
Authorization: Bearer {jwt_token}
```

**Request Body:**
```json
{
  "name": "John Updated",
  "email": "john.updated@example.com",
  "bio": "Senior Software Developer"
}
```

**Response:** (200 OK)
```json
{
  "success": true,
  "data": {
    "_id": "user_id",
    "name": "John Updated",
    "email": "john.updated@example.com",
    "bio": "Senior Software Developer",
    "avatar": ""
  }
}
```

---

## Task Endpoints

All task endpoints require authentication via JWT token in the Authorization header.

### 5. Get All Tasks

**Endpoint:** `GET /tasks`

**Description:** Get all tasks for the authenticated user with optional filters

**Headers:**
```
Authorization: Bearer {jwt_token}
```

**Query Parameters:**
- `status` (optional): Filter by status (pending, in-progress, completed)
- `priority` (optional): Filter by priority (low, medium, high)
- `search` (optional): Search in title and description
- `sortBy` (optional): Field to sort by (default: createdAt)
- `order` (optional): Sort order (asc, desc) (default: desc)

**Example:** `GET /tasks?status=pending&priority=high&search=bug`

**Response:** (200 OK)
```json
{
  "success": true,
  "count": 2,
  "data": [
    {
      "_id": "task_id_1",
      "title": "Fix critical bug",
      "description": "Fix the login bug",
      "status": "in-progress",
      "priority": "high",
      "dueDate": "2024-01-15T00:00:00.000Z",
      "user": "user_id",
      "createdAt": "2024-01-01T00:00:00.000Z",
      "updatedAt": "2024-01-02T00:00:00.000Z"
    }
  ]
}
```

---

### 6. Get Single Task

**Endpoint:** `GET /tasks/:id`

**Description:** Get a specific task by ID

**Headers:**
```
Authorization: Bearer {jwt_token}
```

**Response:** (200 OK)
```json
{
  "success": true,
  "data": {
    "_id": "task_id",
    "title": "Complete documentation",
    "description": "Write API docs",
    "status": "pending",
    "priority": "medium",
    "dueDate": "2024-01-20T00:00:00.000Z",
    "user": "user_id",
    "createdAt": "2024-01-01T00:00:00.000Z",
    "updatedAt": "2024-01-01T00:00:00.000Z"
  }
}
```

---

### 7. Create Task

**Endpoint:** `POST /tasks`

**Description:** Create a new task

**Headers:**
```
Authorization: Bearer {jwt_token}
```

**Request Body:**
```json
{
  "title": "Complete project",
  "description": "Finish the task manager project",
  "status": "pending",
  "priority": "high",
  "dueDate": "2024-01-25T00:00:00.000Z"
}
```

**Validation:**
- `title`: Required, non-empty string
- `description`: Optional string
- `status`: Optional, one of: pending, in-progress, completed (default: pending)
- `priority`: Optional, one of: low, medium, high (default: medium)
- `dueDate`: Optional, ISO 8601 date format

**Response:** (201 Created)
```json
{
  "success": true,
  "data": {
    "_id": "new_task_id",
    "title": "Complete project",
    "description": "Finish the task manager project",
    "status": "pending",
    "priority": "high",
    "dueDate": "2024-01-25T00:00:00.000Z",
    "user": "user_id",
    "createdAt": "2024-01-01T00:00:00.000Z",
    "updatedAt": "2024-01-01T00:00:00.000Z"
  }
}
```

---

### 8. Update Task

**Endpoint:** `PUT /tasks/:id`

**Description:** Update an existing task

**Headers:**
```
Authorization: Bearer {jwt_token}
```

**Request Body:**
```json
{
  "title": "Updated title",
  "status": "completed",
  "priority": "low"
}
```

**Response:** (200 OK)
```json
{
  "success": true,
  "data": {
    "_id": "task_id",
    "title": "Updated title",
    "description": "Original description",
    "status": "completed",
    "priority": "low",
    "dueDate": "2024-01-25T00:00:00.000Z",
    "user": "user_id",
    "createdAt": "2024-01-01T00:00:00.000Z",
    "updatedAt": "2024-01-02T00:00:00.000Z"
  }
}
```

---

### 9. Delete Task

**Endpoint:** `DELETE /tasks/:id`

**Description:** Delete a task

**Headers:**
```
Authorization: Bearer {jwt_token}
```

**Response:** (200 OK)
```json
{
  "success": true,
  "message": "Task deleted successfully"
}
```

---

## Error Responses

All endpoints may return the following error responses:

### 400 Bad Request
```json
{
  "success": false,
  "message": "Validation failed",
  "errors": [
    {
      "msg": "Email is invalid",
      "param": "email"
    }
  ]
}
```

### 401 Unauthorized
```json
{
  "success": false,
  "message": "Not authorized, no token"
}
```

### 403 Forbidden
```json
{
  "success": false,
  "message": "Not authorized to access this resource"
}
```

### 404 Not Found
```json
{
  "success": false,
  "message": "Resource not found"
}
```

### 500 Internal Server Error
```json
{
  "success": false,
  "message": "Server error message"
}
```

---

## Authentication

The API uses JWT (JSON Web Tokens) for authentication. After successful login or signup, you'll receive a token that must be included in the Authorization header for protected routes:

```
Authorization: Bearer {your_jwt_token}
```

Tokens expire after 30 days.

---

## Health Check

**Endpoint:** `GET /health`

**Description:** Check if the server is running

**Response:** (200 OK)
```json
{
  "status": "OK",
  "message": "Server is running"
}
```
