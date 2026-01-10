# Backend API

This is the backend API for API Learning 101. It's a simple RESTful API built with Node.js and Express that performs CRUD operations using a JSON file for data storage (no database required!).

## Features

- ✅ RESTful API design
- ✅ CRUD operations (Create, Read, Update, Delete)
- ✅ JSON file-based storage
- ✅ Input validation
- ✅ Error handling
- ✅ CORS enabled
- ✅ Proper HTTP status codes
- ✅ Ready for Vercel deployment

## Tech Stack

- **Runtime**: Node.js (v18+)
- **Framework**: Express.js
- **Storage**: JSON file (no database!)
- **Middleware**: CORS

## Installation

### Prerequisites

- Node.js 18.0.0 or higher
- npm or yarn

### Steps

1. **Navigate to backend directory**:
```bash
cd backend
```

2. **Install dependencies**:
```bash
npm install
```

3. **Start the server**:
```bash
# Production mode
npm start

# Development mode (with auto-restart)
npm run dev
```

4. **Server running at**:
```
http://localhost:3000
```

## API Endpoints

### Base URL

**Local**: `http://localhost:3000`  
**Production**: `https://api-learning.nisalgunawarchana.com`

### Endpoints

| Method | Endpoint | Description | Status Codes |
|--------|----------|-------------|--------------|
| GET | `/` | API information | 200 |
| GET | `/api/users` | Get all users | 200, 500 |
| GET | `/api/users/:id` | Get user by ID | 200, 404, 500 |
| POST | `/api/users` | Create new user | 201, 400, 409, 422, 500 |
| PUT | `/api/users/:id` | Update user | 200, 400, 404, 409, 422, 500 |
| DELETE | `/api/users/:id` | Delete user | 200, 404, 500 |

## Request/Response Examples

### 1. Get All Users

**Request**:
```http
GET /api/users
```

**Response** (200 OK):
```json
{
  "success": true,
  "count": 3,
  "data": [
    {
      "id": 1,
      "name": "John Doe",
      "email": "john.doe@example.com",
      "age": 30,
      "createdAt": "2026-01-11T08:00:00.000Z"
    }
  ]
}
```

---

### 2. Get User by ID

**Request**:
```http
GET /api/users/1
```

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "id": 1,
    "name": "John Doe",
    "email": "john.doe@example.com",
    "age": 30,
    "createdAt": "2026-01-11T08:00:00.000Z"
  }
}
```

**Response** (404 Not Found):
```json
{
  "success": false,
  "error": "Not Found",
  "message": "User with ID 999 not found"
}
```

---

### 3. Create User

**Request**:
```http
POST /api/users
Content-Type: application/json

{
  "name": "Alice Smith",
  "email": "alice@example.com",
  "age": 28
}
```

**Response** (201 Created):
```json
{
  "success": true,
  "message": "User created successfully",
  "data": {
    "id": 4,
    "name": "Alice Smith",
    "email": "alice@example.com",
    "age": 28,
    "createdAt": "2026-01-11T10:30:00.000Z"
  }
}
```

**Response** (400 Bad Request):
```json
{
  "success": false,
  "error": "Bad Request",
  "message": "Name and email are required fields",
  "requiredFields": ["name", "email"]
}
```

**Response** (409 Conflict):
```json
{
  "success": false,
  "error": "Conflict",
  "message": "User with this email already exists",
  "field": "email"
}
```

**Response** (422 Unprocessable Entity):
```json
{
  "success": false,
  "error": "Validation Error",
  "message": "Invalid email format",
  "field": "email"
}
```

---

### 4. Update User

**Request**:
```http
PUT /api/users/1
Content-Type: application/json

{
  "name": "John Updated",
  "email": "john.updated@example.com",
  "age": 31
}
```

**Response** (200 OK):
```json
{
  "success": true,
  "message": "User updated successfully",
  "data": {
    "id": 1,
    "name": "John Updated",
    "email": "john.updated@example.com",
    "age": 31,
    "createdAt": "2026-01-11T08:00:00.000Z",
    "updatedAt": "2026-01-11T11:00:00.000Z"
  }
}
```

---

### 5. Delete User

**Request**:
```http
DELETE /api/users/1
```

**Response** (200 OK):
```json
{
  "success": true,
  "message": "User deleted successfully",
  "data": {
    "id": 1,
    "name": "John Doe",
    "email": "john.doe@example.com"
  }
}
```

---

## Data Storage

Data is stored in `/data/users.json`. This file is automatically read and written by the API.

**Structure**:
```json
[
  {
    "id": 1,
    "name": "John Doe",
    "email": "john.doe@example.com",
    "age": 30,
    "createdAt": "2026-01-11T08:00:00.000Z"
  }
]
```

## Validation Rules

### Required Fields
- `name` - String, required, cannot be empty
- `email` - String, required, must be valid email format

### Optional Fields
- `age` - Number, optional, must be between 0-150

### Constraints
- Email must be unique
- Email format: `user@domain.com`
- Age must be positive number

## Error Handling

The API uses standard HTTP status codes and returns consistent error responses:

```json
{
  "success": false,
  "error": "Error Type",
  "message": "Human-readable error message",
  "field": "fieldName"  // Optional: which field caused the error
}
```

### Status Codes

| Code | Meaning | Usage |
|------|---------|-------|
| 200 | OK | Successful GET, PUT, DELETE |
| 201 | Created | Successful POST |
| 400 | Bad Request | Missing required fields |
| 404 | Not Found | Resource doesn't exist |
| 409 | Conflict | Duplicate email |
| 422 | Unprocessable Entity | Validation error |
| 500 | Internal Server Error | Server error |

## Testing with cURL

### Get all users
```bash
curl http://localhost:3000/api/users
```

### Get user by ID
```bash
curl http://localhost:3000/api/users/1
```

### Create user
```bash
curl -X POST http://localhost:3000/api/users \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Test User",
    "email": "test@example.com",
    "age": 25
  }'
```

### Update user
```bash
curl -X PUT http://localhost:3000/api/users/1 \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Updated Name",
    "email": "updated@example.com",
    "age": 30
  }'
```

### Delete user
```bash
curl -X DELETE http://localhost:3000/api/users/1
```

## Project Structure

```
backend/
├── api/
│   └── index.js          # Main API code (Express app)
├── data/
│   └── users.json        # JSON file for data storage
├── package.json          # Dependencies and scripts
└── README.md            # This file
```

## Development

### Running in Development Mode

Uses `nodemon` for auto-restart on file changes:

```bash
npm run dev
```

### Running in Production Mode

```bash
npm start
```

## Deployment

This backend is configured for Vercel serverless deployment. See the main project's `vercel.json` for configuration.

## Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| PORT | 3000 | Server port |

## CORS Configuration

CORS is enabled for all origins. In production, you might want to restrict this:

```javascript
app.use(cors({
  origin: 'https://your-frontend-domain.com'
}));
```

## Notes

### Why JSON File Instead of Database?

This is a learning project! Using a JSON file:
- ✅ No database setup required
- ✅ Easy to understand
- ✅ Simple deployment
- ✅ Perfect for learning API concepts

For production applications, use a proper database (PostgreSQL, MongoDB, etc.).

### Limitations

- Not suitable for concurrent writes
- Limited scalability
- No transactions
- Manual backup needed

## Troubleshooting

### Port Already in Use

```bash
# Find process using port 3000
lsof -i :3000

# Kill the process
kill -9 <PID>
```

### Cannot Read/Write File

- Ensure `data/users.json` exists
- Check file permissions
- Verify path is correct

### CORS Errors

- Ensure CORS middleware is enabled
- Check browser console for details
- Verify request headers

## Contributing

See the main [README.md](../README.md) for contribution guidelines.

## License

MIT License - See main project README
