# Idinex

Idinex is organized as a clean monolith with a Go backend, a static frontend, and Docker-based deployment configuration. The project is still scaffold-heavy in places, but the intended structure is already laid out around feature-first backend modules and page/component-based frontend code.

## Project Map

```text
Idinex/
├── .env.example
├── .github/
│   └── workflow/
│       ├── ci.yml
│       ├── lint.yml
│       ├── release.yml
│       └── test.yml
├── Makefile
├── README.md
├── docker-compose.yml
├── backend/
├── deployments/
├── docs/
└── frontend/
```

## Root Files

- `.env.example` lists the environment variables expected by the app.
- `Makefile` documents the project-level commands for setup, development, testing, formatting, database work, Docker builds, and deployment.
- `docker-compose.yml` is the top-level compose file intended to start backend, frontend, and PostgreSQL using Dockerfiles from `deployments/docker/`.
- `README.md` is this project structure guide.
- `.github/workflow/` contains CI, lint, release, and test workflow files.

## Backend

The backend is a Go API module named `idinex-go`.

```text
backend/
├── Dockerfile
├── Makefile
├── go.mod
├── go.sum
├── cmd/
│   └── api/
│       └── main.go
├── config/
│   ├── config.go
│   ├── database.go
│   └── jwt.go
├── internal/
│   ├── access/
│   ├── auth/
│   ├── database/
│   ├── ideas/
│   ├── messaging/
│   ├── middleware/
│   ├── notifications/
│   ├── router/
│   ├── shared/
│   ├── users/
│   └── utils/
├── migrations/
└── tests/
```

### Backend Entry Point

- `backend/cmd/api/main.go` is the API entry point. Its comments show it is intended to load configuration, connect to PostgreSQL, register routes, and start the HTTP server.

### Backend Config

```text
backend/config/
├── config.go
├── database.go
└── jwt.go
```

This package is reserved for application configuration, database configuration, and JWT configuration.

### Backend Feature Modules

Business features live under `backend/internal/`. Most feature modules follow the same layered shape:

```text
feature/
├── dto.go
├── handler.go
├── model.go
├── repository.go
├── routes.go
├── service.go
└── validator.go
```

The layers are intended to work like this:

- `routes.go` registers the module's HTTP endpoints.
- `handler.go` receives HTTP requests and returns HTTP responses.
- `service.go` contains business logic.
- `repository.go` owns database access.
- `model.go` defines internal/domain data models.
- `dto.go` defines request and response shapes.
- `validator.go` validates incoming data.

Current modules:

```text
backend/internal/
├── access/
│   ├── dto.go
│   ├── handler.go
│   ├── model.go
│   ├── repository.go
│   ├── routes.go
│   └── service.go
├── auth/
│   ├── dto.go
│   ├── handler.go
│   ├── jwt.go
│   ├── model.go
│   ├── password.go
│   ├── repository.go
│   ├── routes.go
│   ├── service.go
│   └── validator.go
├── ideas/
│   ├── dto.go
│   ├── hander.go
│   ├── model.go
│   ├── repository.go
│   ├── routes.go
│   ├── service.go
│   └── validator.go
├── messaging/
│   ├── dto.go
│   ├── handler.go
│   ├── model.go
│   ├── repository.go
│   ├── routes.go
│   └── service.go
├── notifications/
│   ├── dto.go
│   ├── handler.go
│   ├── model.go
│   ├── repository.go
│   ├── routes.go
│   └── service.go
└── users/
    ├── dto.go
    ├── hander.go
    ├── model.go
    ├── repository.go
    ├── routes.go
    ├── service.go
    └── validator.go
```

Note: `ideas/hander.go` and `users/hander.go` are currently named `hander.go` in the repo.

### Backend Support Packages

```text
backend/internal/database/
├── postgres.go
└── seed.go

backend/internal/middleware/
├── auth.go
├── cors.go
├── logging.go
├── ratelimit.go
└── recovery.go

backend/internal/router/
├── router.go
└── routes.go

backend/internal/shared/
├── constants.go
├── errors.go
├── helpers.go
├── pagination.go
└── response.go
```

- `database/` is for PostgreSQL connection and seed logic.
- `middleware/` is for request middleware such as auth, CORS, logging, recovery, and rate limiting.
- `router/` is intended to create the app router, attach middleware, and connect every module.
- `shared/` is for common response, pagination, error, constant, and helper code.
- `migrations/`, `tests/`, and `internal/utils/` exist as scaffold directories but currently have no tracked files.

## Frontend

The frontend is organized as static HTML, CSS, and JavaScript.

```text
frontend/
├── README.md
├── index.html
├── assets/
│   ├── fonts/
│   ├── icons/
│   ├── illustrations/
│   ├── images/
│   └── logos/
├── components/
├── css/
├── docs/
├── js/
├── layouts/
└── pages/
```

### Frontend Components

Reusable HTML fragments live in `frontend/components/`.

```text
frontend/components/
├── empty-state.html
├── footer.html
├── idea-card.html
├── modal.html
├── navbar.html
├── notification.html
├── sidebar.html
└── spinner.html
```

### Frontend Styles

CSS is split by responsibility instead of being kept in one large stylesheet.

```text
frontend/css/
├── auth.css
├── base.css
├── buttons.css
├── dashboard.css
├── forms.css
├── ideas.css
├── layout.css
├── navbar.css
├── profile.css
├── responsive.css
├── typography.css
├── utilities.css
└── variables.css
```

### Frontend JavaScript

JavaScript is grouped by API access, feature behavior, reusable components, configuration, routing, and utilities.

```text
frontend/js/
├── api/
│   ├── auth.js
│   ├── client.js
│   ├── ideas.js
│   ├── messages.js
│   ├── notifications.js
│   └── users.js
├── auth/
│   ├── auth-guard.js
│   ├── login.js
│   ├── logout.js
│   ├── register.js
│   └── token-storage.js
├── components/
│   ├── dropdown.js
│   ├── loader.js
│   ├── modal.js
│   ├── navbar.js
│   ├── sidebar.js
│   └── toast.js
├── config/
│   ├── api.js
│   └── environment.js
├── ideas/
│   ├── create.js
│   ├── details.js
│   ├── edit.js
│   ├── list.js
│   └── search.js
├── messages/
│   ├── chat.js
│   ├── conversation.js
│   └── send.js
├── notifications/
│   ├── list.js
│   └── read.js
├── router/
│   └── router.js
├── users/
│   ├── profile.js
│   ├── search.js
│   └── settings.js
└── utils/
    ├── constants.js
    ├── date.js
    ├── debounce.js
    ├── format.js
    ├── storage.js
    └── validation.js
```

### Frontend Layouts

```text
frontend/layouts/
├── auth-layout.html
├── dashboard-layout.html
└── landing-layout.html
```

### Frontend Pages

```text
frontend/pages/
├── 404.html
├── create-idea.html
├── dashboard.html
├── forgot-password.html
├── home.html
├── idea-details.html
├── ideas.html
├── login.html
├── messages.html
├── notifications.html
├── profile.html
├── register.html
└── settings.html
```

### Frontend Assets

```text
frontend/assets/
├── fonts/
├── icons/
├── illustrations/
├── images/
└── logos/
```

These asset folders exist for visual resources. They currently do not contain tracked files.

## Documentation

Project documentation lives in `docs/`.

```text
docs/
├── api.md
├── architecture guide.md
├── authentication.md
├── database.md
├── delployment.md
└── development workflow.md
```

- `architecture guide.md` describes the intended feature-first architecture and request flow.
- `api.md`, `authentication.md`, `database.md`, `delployment.md`, and `development workflow.md` are present as documentation entry points. Some are currently empty or still being drafted.

## Deployments

Deployment configuration is separated from application code.

```text
deployments/
├── README.md
├── development/
│   ├── README.md
│   └── docker-compose.dev.yml
├── docker/
│   ├── backend.Dockerfile
│   ├── frontend.Dockerfile
│   └── postgres.Dockerfile
├── nginx/
│   ├── default.conf
│   └── nginx.conf
└── production/
    ├── .env.production.example
    ├── README.md
    └── docker-compose.prod.yml
```

- `deployments/docker/` contains image build definitions.
- `deployments/nginx/` contains reverse proxy configuration.
- `deployments/development/` contains local development deployment configuration.
- `deployments/production/` contains production deployment configuration.

## Request Flow

The intended backend request flow is:

```text
Browser
  -> Middleware
  -> Handler
  -> Service
  -> Repository
  -> PostgreSQL
```

## Development Commands

The root `Makefile` lists these project commands:

```text
help
setup
dev
stop
restart
build
test
lint
fmt
migrate
rollback
seed
reset-db
clean
logs
backend
frontend
db
docker-build
docker-push
deploy
backup
restore
```

## Current State Notes

- The project structure is in place for backend, frontend, documentation, and deployments.
- Several backend files currently contain package declarations or comments only.
- Some documentation files are placeholders.
- Empty scaffold directories include `backend/migrations/`, `backend/tests/`, `backend/internal/utils/`, `frontend/assets/*`, and `frontend/docs/`.
