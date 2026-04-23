# Setup Guide

Complete setup instructions for the Rinoimob development environment.

## Prerequisites

Before you begin, ensure you have the following installed:

### System Requirements
- **Java 17 or higher** - For backend development
  ```bash
  java -version
  ```

- **Node.js 18 or higher** - For frontend development
  ```bash
  node --version
  npm --version
  ```

- **Docker 20.10+** - For containerized services
  ```bash
  docker --version
  docker-compose --version
  ```

- **Git** - For version control
  ```bash
  git --version
  ```

- **PostgreSQL 12+** - Optional (included in Docker Compose)
- **Maven 3.8.1+** - For Java backend builds

## Installation Steps

### 1. Clone All Repositories

```bash
# Create a workspace directory
mkdir -p ~/workspace/rinoimob
cd ~/workspace/rinoimob

# Clone all repositories
git clone https://github.com/revenlab/rinoimob-backend.git
git clone https://github.com/revenlab/rinoimob-app.git
git clone https://github.com/revenlab/rinoimob-website.git
git clone https://github.com/revenlab/rinoimob-infrastructure.git
git clone https://github.com/revenlab/rinoimob-docs.git
```

### 2. Setup Infrastructure

```bash
cd rinoimob-infrastructure

# Copy environment variables
cp .env.example .env

# Start Docker Compose services
docker-compose up -d

# Verify services are running
docker-compose ps
```

### 3. Setup Backend

```bash
cd ../rinoimob-backend

# Copy environment variables
cp .env.example .env

# Build the project
mvn clean install

# Run tests
mvn test
```

### 4. Setup Frontend App

```bash
cd ../rinoimob-app

# Copy environment variables
cp .env.example .env.local

# Install dependencies
npm install

# Verify the build
npm run build
```

### 5. Setup Website

```bash
cd ../rinoimob-website

# Copy environment variables
cp .env.example .env

# Install dependencies
npm install

# Build the site
npm run build
```

## Environment Variables

Each project has an `.env.example` file. Copy it to `.env` or `.env.local` and configure for your environment.

### Common Variables

#### Backend (.env)
```
DB_URL=jdbc:postgresql://localhost:5432/rinoimob
DB_USER=postgres
DB_PASSWORD=postgres_dev
SPRING_PROFILE=dev
```

#### App (.env.local)
```
VITE_API_URL=http://localhost:8080/api
```

#### Website (.env)
```
NUXT_PUBLIC_API_URL=http://localhost:8080/api
```

## Verification

After completing setup, verify everything works:

```bash
# Backend is running
curl http://localhost:8080

# PostgreSQL is accessible
docker exec rinoimob-postgres psql -U postgres -d rinoimob -c "SELECT 1"

# Redis is accessible
docker exec rinoimob-redis redis-cli ping

# RabbitMQ management UI
open http://localhost:15672  # user: guest, password: guest
```

## Troubleshooting

### Docker containers won't start
```bash
# Check logs
docker-compose logs postgres

# Ensure no port conflicts
lsof -i :5432
lsof -i :6379
lsof -i :5672
```

### Maven build fails
```bash
# Clear Maven cache
rm -rf ~/.m2/repository

# Try rebuild
mvn clean install
```

### Node dependencies conflict
```bash
# Clear npm cache
npm cache clean --force
rm -rf node_modules package-lock.json

# Reinstall
npm install
```

## Next Steps

See [Local Development](./04-LOCAL-DEVELOPMENT.md) to start running the services.
