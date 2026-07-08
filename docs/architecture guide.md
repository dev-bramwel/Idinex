# IDINEX Project Structure Guide

## Philosophy

This project follows **feature-first architecture** and **separation of
concerns**. Every layer has one responsibility.

Request flow:

``` text
Browser
   │
Middleware
   │
Handler
   │
Service
   │
Repository
   │
PostgreSQL
```

## Root Structure

``` text
idinex/
├── backend/
├── frontend/
├── docs/
├── scripts/
├── deployments/
├── .github/
├── docker-compose.yml
├── .env.example
├── README.md
└── LICENSE
```

### backend/

Contains the Go API.

``` text
backend/
├── cmd/
│   └── api/
│       └── main.go
├── configs/
│   ├── config.go
│   ├── database.go
│   └── jwt.go
├── internal/
│   ├── auth/
│   ├── users/
│   ├── ideas/
│   ├── access/
│   ├── messaging/
│   ├── notifications/
│   ├── middleware/
│   ├── database/
│   ├── router/
│   └── shared/
|   └── utils/
├── migrations/
├── tests/
├── docs/
├── Dockerfile
├── Makefile
├── go.mod
└── go.sum
```

## Feature Module Layout

Every business module has the same layout.

``` text
ideas/
├── handler.go
├── service.go
├── repository.go
├── model.go
├── dto.go
├── validator.go
└── routes.go
```

### handler.go

Receives HTTP requests, validates input, calls services and returns
JSON. No SQL and no business rules.

### service.go

Contains all business logic. Decides *what* should happen.

### repository.go

Only database access. SQL queries live here. Services never execute SQL
directly.

### model.go

Represents database entities and tables.

### dto.go

Represents API request and response objects. Prevents exposing internal
models.

### validator.go

Input validation for requests.

### routes.go

Registers module endpoints with the router.

## Authentication

`password.go` - Hash passwords - Compare hashes - Hide hashing
implementation

`jwt.go` - Generate access tokens - Validate tokens - Parse claims -
Refresh tokens

## Middleware

``` text
middleware/
├── auth.go
├── cors.go
├── logging.go
├── recovery.go
└── ratelimit.go
```

Authentication verifies JWTs. Logging records requests. Recovery
prevents crashes from panics. CORS configures cross-origin requests.
Rate limiting protects endpoints from abuse.

## Shared

``` text
shared/
├── constants.go
├── errors.go
├── helpers.go
├── pagination.go
└── response.go
```

Pagination standardizes page, limit and metadata for list endpoints.

## Frontend

``` text
frontend/
├── assets/
├── css/
├── js/
├── pages/
├── components/
├── layouts/
├── images/
├── icons/
└── docs/
```

### CSS

Organized by responsibility rather than one large stylesheet.

### JavaScript

``` text
js/
├── api/
├── auth/
├── ideas/
├── users/
├── messages/
├── notifications/
├── components/
├── utils/
├── config/
└── router/
```

API files communicate with the backend only. Feature folders manipulate
UI. Utilities provide reusable helpers.

## Development Workflow

-   Never commit directly to `main`.
-   Create feature branches from `develop`.
-   Open Pull Requests.
-   Require review before merging.
-   Merge tested work into `develop`.
-   Release from `main`.

## Guiding Principles

-   One responsibility per file.
-   One responsibility per layer.
-   Stateless backend.
-   Feature-first organization.
-   Keep modules independent.
-   Build a clean monolith first.
-   Add complexity only when justified by product growth.
