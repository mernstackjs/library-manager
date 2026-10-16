# Architecture

## Backend Structure

backend/
├── src/
│   ├── config/
│   ├── routes/
│   ├── controllers/
│   ├── services/
│   ├── middleware/
│   ├── validators/
│   ├── utils/
│   ├── app.js
│   └── server.js

## Responsibilities

### config
Database, Redis and environment configuration.

### routes
Defines API paths and HTTP methods.

### controllers
Handles request and response.

### services
Contains business logic and database operations.

### middleware
Authentication, authorization and error handling.

### validators
Validates incoming request data.

### utils
Reusable helper functions.

## Request Flow

Client
→ Route
→ Middleware
→ Controller
→ Service
→ PostgreSQL / Redis
→ Controller
→ Response

## Main Modules

- Auth
- Books
- Members
- Loans
- Dashboard

## Database
PostgreSQL is the source of truth.

## Cache
Redis will be added after the core API works.

## Real-Time
Socket.IO will be added after the frontend and core backend are stable.

## Infrastructure
Docker will run the services.
Nginx will be the public entry point.

## Error Strategy
Use consistent API errors:
- 400 invalid input
- 401 unauthenticated
- 403 forbidden
- 404 not found
- 409 conflict
- 500 server error

## Development Rule
Build the project in layers.
Do not add Redis, Socket.IO, Docker or Nginx before the core backend works.