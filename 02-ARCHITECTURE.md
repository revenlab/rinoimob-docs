# Architecture

System design and technical architecture of the Rinoimob property management platform.

## Overview

Rinoimob is a multi-repository, microservices-ready property management SaaS platform with a focus on on-premise deployment.

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                      Client Layer                               │
├──────────────────────┬──────────────────────┬───────────────────┤
│  Web App (Vue 3)     │  Website (Nuxt 3)    │   Mobile (Future) │
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
│            Port: 8080                                           │
│            ├── REST API                                         │
│            ├── Authentication                                   │
│            └── Business Logic                                   │
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
│   ├── src/main/java/
│   ├── src/test/java/
│   ├── pom.xml
│   └── .github/workflows/ci.yml
│
├── rinoimob-app/               # Vue 3 Frontend Application
│   ├── src/
│   ├── package.json
│   └── .github/workflows/ci.yml
│
├── rinoimob-website/           # Nuxt 3 Website (SSR)
│   ├── pages/
│   ├── package.json
│   └── .github/workflows/ci.yml
│
├── rinoimob-infrastructure/    # Docker Compose & IaC
│   ├── docker-compose.yml
│   ├── scripts/
│   └── .github/workflows/ci.yml
│
└── rinoimob-docs/              # Documentation
    ├── 01-SETUP.md
    ├── 02-ARCHITECTURE.md
    ├── 03-CODING-STANDARDS.md
    ├── 04-LOCAL-DEVELOPMENT.md
    └── 05-CONTRIBUTION-GUIDE.md
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

- Spring Security for authentication/authorization
- JWT tokens for API security
- Environment variables for sensitive data
- Database credentials managed via .env files
- CORS configuration for frontend communication
- Password hashing with bcrypt

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
