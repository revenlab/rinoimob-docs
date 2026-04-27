# Architecture

System design and technical architecture of the Rinoimob property management platform.

## Overview

Rinoimob is a multi-repository, microservices-ready property management SaaS platform with a focus on on-premise deployment.

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                      Client Layer                               │
├──────────────────────┬──────────────────────┬───────────────────┤
│  Admin App (Vue 3)   │  Website (Nuxt 3)    │   Mobile (Future) │
│  Fixed domain        │  Tenant subdomain    │                   │
│  Port: 5173          │  Port: 3000          │                   │
└──────────────────────┴──────────────────────┴───────────────────┘
                              ↓
                         ┌──────────┐
                         │ Nginx/   │
                         │ API GW   │
                         └──────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                    API Layer                                    │
│            Spring Boot 3 Backend (Java 17)                      │
│            Port: 39000                                           │
│            ├── REST API (/api/v1/*)                             │
│            ├── Authentication (/api/auth/*)                     │
│            ├── TenantContext per request (ThreadLocal)          │
│            └── Business Logic (tenant-scoped queries)           │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                  Infrastructure Layer                           │
├──────────────────┬──────────────────┬────────────────────────────┤
│  PostgreSQL 15   │  Redis 7         │  RabbitMQ 3.12             │
│  Database        │  Cache/Sessions  │  Message Broker            │
│  Port: 5432      │  Port: 6379      │  Ports: 5672, 15672       │
└──────────────────┴──────────────────┴────────────────────────────┘
```

## Multi-Tenant Model

Rinoimob uses a **shared-schema, row-level multi-tenancy** approach:

- All tenants share the same database schema
- Every business table has a `tenant_id UUID NOT NULL` column
- All queries are scoped by `tenant_id` — enforced at the repository layer
- `TenantContext` (ThreadLocal) carries the resolved tenant UUID for the duration of each request

See [07-MULTITENANT-AUTH.md](./07-MULTITENANT-AUTH.md) for the complete model.

### Domain Routing

| Domain type | Who uses it | Example |
|---|---|---|
| Fixed admin domain | Imobiliária staff / admin panel | `app.rinoimob.com` |
| Tenant subdomain | Client-facing website of the imobiliária | `acme.rinoimob.com` |
| Custom domain (future) | Tenant's own branding | `imoveis.acme.com.br` |

## Technology Stack

### Backend
- **Framework**: Spring Boot 3.x
- **Language**: Java 17+
- **Database**: PostgreSQL 15
- **Build Tool**: Maven
- **Testing**: JUnit 5, AssertJ
- **Security**: Spring Security with JWT

### Frontend App
- **Framework**: Vue 3
- **Build Tool**: Vite
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **State Management**: Pinia
- **Routing**: Vue Router

### Website
- **Framework**: Nuxt 3 (SSR)
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **Static Generation**: Supported

### Infrastructure
- **Containerization**: Docker
- **Orchestration**: Docker Compose (development), Kubernetes (future)
- **Database**: PostgreSQL 15
- **Cache**: Redis 7
- **Message Queue**: RabbitMQ 3.12

## Repository Structure

```
rinoimob/
├── rinoimob-backend/           # Spring Boot REST API
│   ├── src/main/java/com/rinoimob/
│   │   ├── api/controller/     # REST controllers
│   │   ├── config/             # Spring config, security, CORS
│   │   ├── domain/
│   │   │   ├── dto/            # Request/Response DTOs (records)
│   │   │   ├── entity/         # JPA entities
│   │   │   └── repository/     # Spring Data JPA repositories
│   │   ├── interceptor/        # TenantInterceptor (subdomain fallback)
│   │   ├── service/auth/       # AuthService, JwtTokenProvider
│   │   ├── tenant/             # TenantContext (ThreadLocal)
│   │   └── validation/         # @ValidPassword constraint
│   ├── src/test/java/
│   ├── pom.xml
│   └── .github/workflows/ci.yml
│
├── rinoimob-app/               # Vue 3 Admin Frontend
│   ├── src/
│   │   ├── components/
│   │   │   ├── auth/           # LoginForm, RegisterForm, etc.
│   │   │   └── ui/             # PasswordStrength, shared UI
│   │   ├── composables/        # usePasswordStrength, useAuth
│   │   ├── layouts/            # AppLayout (sidebar shell)
│   │   ├── stores/             # Pinia stores (auth, workspace)
│   │   ├── views/              # Dashboard, Profile, ChangePassword
│   │   └── router/             # Vue Router config
│   ├── package.json
│   └── .github/workflows/ci.yml
│
├── rinoimob-website/           # Nuxt 3 Website (SSR, per-tenant)
│   ├── pages/
│   ├── package.json
│   └── .github/workflows/ci.yml
│
├── rinoimob-infrastructure/    # Docker Compose & IaC
│   ├── docker-compose.yml
│   ├── .env.example
│   └── scripts/
│
└── rinoimob-docs/              # Documentation
    ├── 01-SETUP.md
    ├── 02-ARCHITECTURE.md
    ├── 03-CODING-STANDARDS.md
    ├── 04-LOCAL-DEVELOPMENT.md
    ├── 05-CONTRIBUTION-GUIDE.md
    ├── 06-DESIGN-SYSTEM-GLASSMORPHISM.md
    └── 07-MULTITENANT-AUTH.md
```

## Deployment Strategy

### Development
- Docker Compose for local services
- Hot reload for code changes
- Development databases with sample data

### Testing
- Automated testing in CI/CD pipeline
- Database migrations verified
- Container image building validated

### Production (Planned)
- Kubernetes for orchestration
- Persistent volumes for data
- Load balancing and auto-scaling
- Environment-specific configurations

## Security Considerations

- Spring Security + JWT for authentication/authorization
- **Two-step login**: identify → workspace select → scoped JWT (see [07-MULTITENANT-AUTH.md](./07-MULTITENANT-AUTH.md))
- JWT secret must be **≥ 64 ASCII characters** (512 bits for HS512)
- `TenantContext` set by `JwtAuthenticationFilter`; `TenantInterceptor` only acts as fallback for unauthenticated routes
- All repository queries filtered by `tenant_id`
- **Password policy**: min 6 chars, uppercase, lowercase, digit, special char (`@$!%*?&_-#^`)
- Password hashing with **bcrypt** via Spring Security `BCryptPasswordEncoder`
- Email verification required before login
- Environment variables for all secrets — never commit credentials

## Scalability

The architecture supports:
- Horizontal scaling of backend services
- Database read replicas
- Cache layer for performance
- Asynchronous processing via message queue
- CDN integration for static assets

## Data Flow

1. **Client Request**: User interacts with Vue app or visits Nuxt website
2. **API Call**: Frontend sends HTTP request to Spring Boot backend
3. **Processing**: Backend processes request, queries database, caches data
4. **Response**: API returns JSON response to client
5. **State Update**: Frontend updates UI with new data

## Future Enhancements

- Microservices migration
- Kubernetes deployment
- API versioning
- GraphQL support
- Mobile app (React Native)
- Advanced analytics
- Real-time notifications via WebSocket
