# Go API with PostgreSQL and GORM

A simple REST API built with Go, Gin, PostgreSQL, and GORM with Swagger documentation.

## Project Structure

```
go-api/
├── config/
│   └── config.go
├── database/
│   └── connection.go
├── handlers/
│   ├── user.go
│   └── health.go
├── models/
│   └── user.go
├── repository/
│   └── user_repository.go
├── services/
│   └── user_service.go
├── docs/
│   └── (swagger generated files)
├── main.go
├── go.mod
├── go.sum
├── Dockerfile
├── .env
└── README.md
```

## Features

- RESTful API with CRUD operations for users
- Clean architecture with separated layers
- PostgreSQL database with GORM ORM
- Auto-generated Swagger documentation
- Environment-based configuration
- Health check endpoint

## Prerequisites

- Go 1.21+
- PostgreSQL database (already set up)

## Installation

1. **Your project structure is already set up correctly!**

2. **Initialize Go modules (if not done already):**

   ```bash
   go mod tidy
   ```

3. **Install Swagger CLI:**

   ```bash
   go install github.com/swaggo/swag/cmd/swag@latest
   ```

4. **Update your .env file with your PostgreSQL connection details:**

   ```env
   DATABASE_URL=postgres://username:password@localhost:5432/your_database_name?sslmode=disable
   PORT=8080
   ```

5. **Generate Swagger documentation:**
   ```bash
   swag init
   ```

## Running the Application

Since you already have PostgreSQL set up, simply run:

```bash
go run main.go
```

The application will:

- Connect to your existing PostgreSQL database
- Auto-migrate the User table
- Start the server on port 8080

## API Endpoints

- **Health Check**: `GET /health`
- **Get All Users**: `GET /api/v1/users`
- **Get User by ID**: `GET /api/v1/users/{id}`
- **Create User**: `POST /api/v1/users`
- **Update User**: `PUT /api/v1/users/{id}`
- **Delete User**: `DELETE /api/v1/users/{id}`

## API Documentation

Once the server is running, you can access the Swagger documentation at:

- http://localhost:8080/swagger/index.html

## Example Usage

### Create a User

```bash
curl -X POST http://localhost:8080/api/v1/users \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe",
    "email": "john@example.com",
    "age": 30
  }'
```

### Get All Users

```bash
curl http://localhost:8080/api/v1/users
```

### Get User by ID

```bash
curl http://localhost:8080/api/v1/users/1
```

### Update User

```bash
curl -X PUT http://localhost:8080/api/v1/users/1 \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Smith",
    "age": 31
  }'
```

### Delete User

```bash
curl -X DELETE http://localhost:8080/api/v1/users/1
```

## Database Schema

The User model includes:

- `id` (uint, primary key)
- `name` (string, required)
- `email` (string, required, unique)
- `age` (int)
- `created_at` (timestamp)
- `updated_at` (timestamp)
- `deleted_at` (timestamp, for soft delete)

## Technologies Used

- **Go**: Programming language
- **Gin**: HTTP web framework
- **GORM**: Object-Relational Mapping library
- **PostgreSQL**: Database
- **Swagger**: API documentation

## Setup Steps for Your Structure

1. Make sure all files are copied to their respective directories
2. Update the `.env` file with your PostgreSQL credentials
3. Run: `go mod tidy`
4. Run: `go install github.com/swaggo/swag/cmd/swag@latest`
5. Run: `swag init`
6. Run: `go run main.go`

Your API will be available at `http://localhost:8080` with Swagger docs at `/swagger/index.html`!

## Notes for Your Setup

- Since you're using an existing PostgreSQL database, just update the `DATABASE_URL` in the `.env` file
- The application will automatically create the `users` table when it starts (auto-migration)
- No Docker Compose needed since you already have PostgreSQL running
- The flat structure you chose works perfectly and keeps things simple/users/{id}`
- **Create User**: `POST /api/v1/users`
- **Update User**: `PUT /api/v1/users/{id}`
- **Delete User**: `DELETE /api/v1/users/{id}`

## API Documentation

Once the server is running, you can access the Swagger documentation at:

- http://localhost:8080/swagger/index.html

## Example Usage

### Create a User

```bash
curl -X POST http://localhost:8080/api/v1/users \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe",
    "email": "john@example.com",
    "age": 30
  }'
```

### Get All Users

```bash
curl http://localhost:8080/api/v1/users
```

### Get User by ID

```bash
curl http://localhost:8080/api/v1/users/1
```

### Update User

```bash
curl -X PUT http://localhost:8080/api/v1/users/1 \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Smith",
    "age": 31
  }'
```

### Delete User

```bash
curl -X DELETE http://localhost:8080/api/v1/users/1
```

## Environment Variables

Create a `.env` file in the root directory:

```env
DATABASE_URL=postgres://user:password@localhost:5432/apidb?sslmode=disable
PORT=8080
```

## Database Schema

The User model includes:

- `id` (uint, primary key)
- `name` (string, required)
- `email` (string, required, unique)
- `age` (int)
- `created_at` (timestamp)
- `updated_at` (timestamp)
- `deleted_at` (timestamp, for soft delete)

## Technologies Used

- **Go**: Programming language
- **Gin**: HTTP web framework
- **GORM**: Object-Relational Mapping library
- **PostgreSQL**: Database
- **Swagger**: API documentation
- **Docker**: Containerization

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request
