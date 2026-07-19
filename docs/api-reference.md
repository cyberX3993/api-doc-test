# API Reference

## Authentication

### POST /auth/login

Authenticate a user and issue JWT tokens.

- URL: `/auth/login`
- Method: `POST`
- Required Permissions: Public
- Headers:
  - `Content-Type: application/json`
- Request Body:
  - `email` (string, required)
  - `password` (string, required)

#### Request Example

```json
{
  "email": "admin@school.example.com",
  "password": "Secur3Pass!"
}
```

#### Success Response

- Status: `201 Created`

```json
{
  "accessToken": "eyJhbGci...",
  "refreshToken": "def502...",
  "expiresIn": 3600,
  "tokenType": "Bearer"
}
```

#### Error Responses

- `400 Bad Request`
- `401 Unauthorized`
- `422 Unprocessable Entity`

#### cURL Example

```bash
curl https://api.school.example.com/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"admin@school.example.com","password":"Secur3Pass!"}'
```

#### JavaScript Fetch Example

```js
const response = await fetch('https://api.school.example.com/v1/auth/login', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    email: 'admin@school.example.com',
    password: 'Secur3Pass!'
  })
});
const data = await response.json();
```

#### Python Requests Example

```py
import requests

response = requests.post(
  'https://api.school.example.com/v1/auth/login',
  json={
    'email': 'admin@school.example.com',
    'password': 'Secur3Pass!'
  }
)
response.raise_for_status()
print(response.json())
```

---

## Students

### GET /students

Return a paginated list of students.

- URL: `/students`
- Method: `GET`
- Required Permissions: `read:students`
- Headers:
  - `Authorization: Bearer <access_token>`
  - `Accept: application/json`
- Query Parameters:
  - `page` (integer, optional)
  - `limit` (integer, optional)
  - `search` (string, optional)
  - `status` (string, optional) — `ACTIVE`, `INACTIVE`, `DELETED`
  - `sort` (string, optional) — e.g. `firstName`, `-createdAt`

#### Example Request

```http
GET /students?page=1&limit=20&search=John&status=ACTIVE&sort=lastName HTTP/1.1
Authorization: Bearer <access_token>
Accept: application/json
```

#### Success Response

- Status: `200 OK`

```json
{
  "data": [
    {
      "id": "a6e55c90-f8b2-4a64-b3d6-a1d95b7d45fa",
      "firstName": "John",
      "lastName": "Doe",
      "email": "john@example.com",
      "phone": "+1-555-123-4567",
      "dateOfBirth": "2010-03-18",
      "gender": "Male",
      "courseId": "course-001",
      "status": "ACTIVE",
      "createdAt": "2026-01-01T12:00:00Z",
      "updatedAt": "2026-01-01T12:00:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "totalItems": 235,
    "totalPages": 12,
    "hasNext": true,
    "hasPrevious": false
  }
}
```

#### Error Responses

- `400 Bad Request`
- `401 Unauthorized`
- `403 Forbidden`
- `429 Too Many Requests`

#### cURL Example

```bash
curl "https://api.school.example.com/v1/students?page=1&limit=20&status=ACTIVE" \
  -H "Authorization: Bearer <access_token>"
```

#### JavaScript Fetch Example

```js
const response = await fetch('https://api.school.example.com/v1/students?page=1&limit=20&status=ACTIVE', {
  headers: {
    Authorization: 'Bearer <access_token>'
  }
});
const payload = await response.json();
```

#### Python Requests Example

```py
import requests

response = requests.get(
  'https://api.school.example.com/v1/students',
  headers={
    'Authorization': 'Bearer <access_token>'
  },
  params={
    'page': 1,
    'limit': 20,
    'status': 'ACTIVE'
  }
)
response.raise_for_status()
print(response.json())
```

---

### POST /students

Create a new student.

- URL: `/students`
- Method: `POST`
- Required Permissions: `write:students`
- Headers:
  - `Authorization: Bearer <access_token>`
  - `Content-Type: application/json`
- Request Body:
  - `firstName` (string, required)
  - `lastName` (string, required)
  - `email` (string, required)
  - `phone` (string, optional)
  - `dateOfBirth` (string, required, format: `YYYY-MM-DD`)
  - `gender` (string, optional) — `Male`, `Female`, `Other`
  - `courseId` (string, required)
  - `status` (string, optional) — default `ACTIVE`

#### Validation Rules

- `firstName`, `lastName`, `email`, `dateOfBirth`, `courseId` are required
- `email` must be a valid email address
- `dateOfBirth` must be ISO8601 date format
- `status` must be one of `ACTIVE`, `INACTIVE`, `DELETED`

#### Request Example

```json
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john@example.com",
  "phone": "+1-555-123-4567",
  "dateOfBirth": "2010-03-18",
  "gender": "Male",
  "courseId": "course-001",
  "status": "ACTIVE"
}
```

#### Success Response

- Status: `201 Created`

```json
{
  "id": "a6e55c90-f8b2-4a64-b3d6-a1d95b7d45fa",
  "firstName": "John",
  "lastName": "Doe",
  "email": "john@example.com",
  "phone": "+1-555-123-4567",
  "dateOfBirth": "2010-03-18",
  "gender": "Male",
  "courseId": "course-001",
  "status": "ACTIVE",
  "createdAt": "2026-01-01T12:00:00Z",
  "updatedAt": "2026-01-01T12:00:00Z"
}
```

#### Error Responses

- `400 Bad Request`
- `401 Unauthorized`
- `403 Forbidden`
- `409 Conflict`
- `422 Unprocessable Entity`

#### cURL Example

```bash
curl https://api.school.example.com/v1/students \
  -H "Authorization: Bearer <access_token>" \
  -H "Content-Type: application/json" \
  -d '{"firstName":"John","lastName":"Doe","email":"john@example.com","phone":"+1-555-123-4567","dateOfBirth":"2010-03-18","gender":"Male","courseId":"course-001","status":"ACTIVE"}'
```

#### JavaScript Fetch Example

```js
const response = await fetch('https://api.school.example.com/v1/students', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    Authorization: 'Bearer <access_token>'
  },
  body: JSON.stringify({
    firstName: 'John',
    lastName: 'Doe',
    email: 'john@example.com',
    phone: '+1-555-123-4567',
    dateOfBirth: '2010-03-18',
    gender: 'Male',
    courseId: 'course-001',
    status: 'ACTIVE'
  })
});
const data = await response.json();
```

#### Python Requests Example

```py
import requests

response = requests.post(
  'https://api.school.example.com/v1/students',
  headers={
    'Authorization': 'Bearer <access_token>',
    'Content-Type': 'application/json'
  },
  json={
    'firstName': 'John',
    'lastName': 'Doe',
    'email': 'john@example.com',
    'phone': '+1-555-123-4567',
    'dateOfBirth': '2010-03-18',
    'gender': 'Male',
    'courseId': 'course-001',
    'status': 'ACTIVE'
  }
)
response.raise_for_status()
print(response.json())
```

---

### GET /students/{studentId}

Return a single student profile.

- URL: `/students/{studentId}`
- Method: `GET`
- Required Permissions: `read:students`
- Headers:
  - `Authorization: Bearer <access_token>`
- Path Parameters:
  - `studentId` (string, required)

#### Success Response

- Status: `200 OK`

```json
{
  "id": "a6e55c90-f8b2-4a64-b3d6-a1d95b7d45fa",
  "firstName": "John",
  "lastName": "Doe",
  "email": "john@example.com",
  "phone": "+1-555-123-4567",
  "dateOfBirth": "2010-03-18",
  "gender": "Male",
  "courseId": "course-001",
  "status": "ACTIVE",
  "createdAt": "2026-01-01T12:00:00Z",
  "updatedAt": "2026-01-01T12:00:00Z"
}
```

#### Error Responses

- `401 Unauthorized`
- `403 Forbidden`
- `404 Not Found`

#### cURL Example

```bash
curl https://api.school.example.com/v1/students/a6e55c90-f8b2-4a64-b3d6-a1d95b7d45fa \
  -H "Authorization: Bearer <access_token>"
```

#### JavaScript Fetch Example

```js
const response = await fetch('https://api.school.example.com/v1/students/a6e55c90-f8b2-4a64-b3d6-a1d95b7d45fa', {
  headers: {
    Authorization: 'Bearer <access_token>'
  }
});
const student = await response.json();
```

#### Python Requests Example

```py
import requests

response = requests.get(
  'https://api.school.example.com/v1/students/a6e55c90-f8b2-4a64-b3d6-a1d95b7d45fa',
  headers={
    'Authorization': 'Bearer <access_token>'
  }
)
response.raise_for_status()
print(response.json())
```

---

### PUT /students/{studentId}

Update student information.

- URL: `/students/{studentId}`
- Method: `PUT`
- Required Permissions: `write:students`
- Headers:
  - `Authorization: Bearer <access_token>`
  - `Content-Type: application/json`
- Path Parameters:
  - `studentId` (string, required)
- Request Body:
  - `firstName` (string, required)
  - `lastName` (string, required)
  - `email` (string, required)
  - `phone` (string, optional)
  - `dateOfBirth` (string, required, format: `YYYY-MM-DD`)
  - `gender` (string, optional)
  - `courseId` (string, required)
  - `status` (string, optional)

#### Success Response

- Status: `200 OK`

```json
{
  "id": "a6e55c90-f8b2-4a64-b3d6-a1d95b7d45fa",
  "firstName": "John",
  "lastName": "Doe",
  "email": "john@example.com",
  "phone": "+1-555-123-4567",
  "dateOfBirth": "2010-03-18",
  "gender": "Male",
  "courseId": "course-001",
  "status": "ACTIVE",
  "createdAt": "2026-01-01T12:00:00Z",
  "updatedAt": "2026-07-19T08:00:00Z"
}
```

#### Error Responses

- `400 Bad Request`
- `401 Unauthorized`
- `403 Forbidden`
- `404 Not Found`
- `422 Unprocessable Entity`

#### cURL Example

```bash
curl https://api.school.example.com/v1/students/a6e55c90-f8b2-4a64-b3d6-a1d95b7d45fa \
  -X PUT \
  -H "Authorization: Bearer <access_token>" \
  -H "Content-Type: application/json" \
  -d '{"firstName":"John","lastName":"Doe","email":"john@example.com","phone":"+1-555-123-4567","dateOfBirth":"2010-03-18","gender":"Male","courseId":"course-001","status":"ACTIVE"}'
```

#### JavaScript Fetch Example

```js
const response = await fetch('https://api.school.example.com/v1/students/a6e55c90-f8b2-4a64-b3d6-a1d95b7d45fa', {
  method: 'PUT',
  headers: {
    'Content-Type': 'application/json',
    Authorization: 'Bearer <access_token>'
  },
  body: JSON.stringify({
    firstName: 'John',
    lastName: 'Doe',
    email: 'john@example.com',
    phone: '+1-555-123-4567',
    dateOfBirth: '2010-03-18',
    gender: 'Male',
    courseId: 'course-001',
    status: 'ACTIVE'
  })
});
const updatedStudent = await response.json();
```

#### Python Requests Example

```py
import requests

response = requests.put(
  'https://api.school.example.com/v1/students/a6e55c90-f8b2-4a64-b3d6-a1d95b7d45fa',
  headers={
    'Authorization': 'Bearer <access_token>',
    'Content-Type': 'application/json'
  },
  json={
    'firstName': 'John',
    'lastName': 'Doe',
    'email': 'john@example.com',
    'phone': '+1-555-123-4567',
    'dateOfBirth': '2010-03-18',
    'gender': 'Male',
    'courseId': 'course-001',
    'status': 'ACTIVE'
  }
)
response.raise_for_status()
print(response.json())
```

---

### DELETE /students/{studentId}

Soft delete a student record.

- URL: `/students/{studentId}`
- Method: `DELETE`
- Required Permissions: `write:students`
- Headers:
  - `Authorization: Bearer <access_token>`
- Path Parameters:
  - `studentId` (string, required)

#### Success Response

- Status: `204 No Content`

#### Error Responses

- `401 Unauthorized`
- `403 Forbidden`
- `404 Not Found`

#### cURL Example

```bash
curl -X DELETE https://api.school.example.com/v1/students/a6e55c90-f8b2-4a64-b3d6-a1d95b7d45fa \
  -H "Authorization: Bearer <access_token>"
```

#### JavaScript Fetch Example

```js
await fetch('https://api.school.example.com/v1/students/a6e55c90-f8b2-4a64-b3d6-a1d95b7d45fa', {
  method: 'DELETE',
  headers: {
    Authorization: 'Bearer <access_token>'
  }
});
```

#### Python Requests Example

```py
import requests

response = requests.delete(
  'https://api.school.example.com/v1/students/a6e55c90-f8b2-4a64-b3d6-a1d95b7d45fa',
  headers={
    'Authorization': 'Bearer <access_token>'
  }
)
response.raise_for_status()
```

---

## Courses

### GET /courses

Return available courses.

- URL: `/courses`
- Method: `GET`
- Required Permissions: `read:courses`
- Headers:
  - `Authorization: Bearer <access_token>`

#### Success Response

- Status: `200 OK`

```json
{
  "data": [
    {
      "id": "course-001",
      "name": "Computer Science",
      "duration": "4 Years"
    },
    {
      "id": "course-002",
      "name": "Mathematics",
      "duration": "3 Years"
    }
  ]
}
```

#### Error Responses

- `401 Unauthorized`
- `403 Forbidden`

#### cURL Example

```bash
curl https://api.school.example.com/v1/courses \
  -H "Authorization: Bearer <access_token>"
```

#### JavaScript Fetch Example

```js
const response = await fetch('https://api.school.example.com/v1/courses', {
  headers: {
    Authorization: 'Bearer <access_token>'
  }
});
const courses = await response.json();
```

#### Python Requests Example

```py
import requests

response = requests.get(
  'https://api.school.example.com/v1/courses',
  headers={
    'Authorization': 'Bearer <access_token>'
  }
)
response.raise_for_status()
print(response.json())
```

---

## Attendance

### POST /attendance

Record attendance for a student.

- URL: `/attendance`
- Method: `POST`
- Required Permissions: `write:attendance`
- Headers:
  - `Authorization: Bearer <access_token>`
  - `Content-Type: application/json`
- Request Body:
  - `studentId` (string, required)
  - `date` (string, required, format: `YYYY-MM-DD`)
  - `status` (string, required) — `Present`, `Absent`, `Late`, `Excused`

#### Validation Rules

- `studentId` must be a valid UUID
- `date` must be ISO8601 date format
- `status` must be one of `Present`, `Absent`, `Late`, `Excused`

#### Request Example

```json
{
  "studentId": "a6e55c90-f8b2-4a64-b3d6-a1d95b7d45fa",
  "date": "2026-07-20",
  "status": "Present"
}
```

#### Success Response

- Status: `201 Created`

```json
{
  "studentId": "a6e55c90-f8b2-4a64-b3d6-a1d95b7d45fa",
  "date": "2026-07-20",
  "status": "Present"
}
```

#### Error Responses

- `400 Bad Request`
- `401 Unauthorized`
- `403 Forbidden`
- `404 Not Found`
- `422 Unprocessable Entity`

#### cURL Example

```bash
curl https://api.school.example.com/v1/attendance \
  -H "Authorization: Bearer <access_token>" \
  -H "Content-Type: application/json" \
  -d '{"studentId":"a6e55c90-f8b2-4a64-b3d6-a1d95b7d45fa","date":"2026-07-20","status":"Present"}'
```

#### JavaScript Fetch Example

```js
const response = await fetch('https://api.school.example.com/v1/attendance', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    Authorization: 'Bearer <access_token>'
  },
  body: JSON.stringify({
    studentId: 'a6e55c90-f8b2-4a64-b3d6-a1d95b7d45fa',
    date: '2026-07-20',
    status: 'Present'
  })
});
const attendance = await response.json();
```

#### Python Requests Example

```py
import requests

response = requests.post(
  'https://api.school.example.com/v1/attendance',
  headers={
    'Authorization': 'Bearer <access_token>',
    'Content-Type': 'application/json'
  },
  json={
    'studentId': 'a6e55c90-f8b2-4a64-b3d6-a1d95b7d45fa',
    'date': '2026-07-20',
    'status': 'Present'
  }
)
response.raise_for_status()
print(response.json())
```
