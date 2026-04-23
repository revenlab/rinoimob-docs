# Local Development Guide

Guide for running and developing the Rinoimob platform locally.

## Prerequisites

See [Setup Guide](./01-SETUP.md) for initial setup.

## Running Services

### 1. Start Infrastructure

```bash
cd rinoimob-infrastructure

# Copy environment if not done
cp .env.example .env

# Start all services
docker-compose up -d

# Verify services
docker-compose ps

# View logs
docker-compose logs -f
```

Services will be available at:
- PostgreSQL: `localhost:5432`
- Redis: `localhost:6379`
- RabbitMQ: `localhost:5672` (AMQP) / `localhost:15672` (Management UI)

### 2. Run Backend

```bash
cd ../rinoimob-backend

# Copy environment
cp .env.example .env

# Build and run
mvn clean spring-boot:run
```

Backend will be available at:
- API: `http://localhost:8080`
- **Swagger UI**: `http://localhost:8080/swagger-ui.html`
- **OpenAPI Spec**: `http://localhost:8080/v3/api-docs`

Check if running:
```bash
curl http://localhost:8080
```

### 3. Run Frontend App

```bash
cd ../rinoimob-app

# Copy environment
cp .env.example .env.local

# Install dependencies
npm install

# Start development server
npm run dev
```

App will be available at: `http://localhost:5173`

### 4. Run Website

```bash
cd ../rinoimob-website

# Copy environment
cp .env.example .env

# Install dependencies
npm install

# Start development server
npm run dev
```

Website will be available at: `http://localhost:3000`

## Development Workflow

### Terminal Setup

Recommended setup with multiple terminals:

**Terminal 1 - Infrastructure**
```bash
cd rinoimob-infrastructure
docker-compose up
```

**Terminal 2 - Backend**
```bash
cd rinoimob-backend
mvn spring-boot:run
```

**Terminal 3 - Frontend App**
```bash
cd rinoimob-app
npm run dev
```

**Terminal 4 - Website**
```bash
cd rinoimob-website
npm run dev
```

### Hot Reload

All services support hot reload:
- **Backend**: Spring Boot DevTools (automatically restarts on file changes)
- **App**: Vite (updates instantly in browser)
- **Website**: Nuxt (updates instantly in browser)

Just save your changes and see results immediately!

## Database Management

### Accessing PostgreSQL

```bash
# Connect to PostgreSQL
docker exec -it rinoimob-postgres psql -U postgres -d rinoimob

# Common commands
\dt                    # List tables
\d table_name         # Describe table
SELECT * FROM table;  # Query data
```

### Reset Database

```bash
# Stop PostgreSQL
docker-compose stop postgres

# Remove volume
docker volume rm rinoimob-infrastructure_postgres_data

# Start PostgreSQL
docker-compose up -d postgres

# Run initialization script
docker exec -i rinoimob-postgres psql -U postgres -d rinoimob < scripts/init-db.sql
```

## Redis Management

### Connect to Redis

```bash
# Open Redis CLI
docker exec -it rinoimob-redis redis-cli

# Common commands
PING                  # Check connection
KEYS *               # List all keys
GET key_name         # Get value
DEL key_name         # Delete key
FLUSHDB              # Clear all keys
```

## RabbitMQ Management

### Access Management UI

Open browser to: `http://localhost:15672`

Default credentials:
- Username: `guest`
- Password: `guest`

### Common Operations

- View queues and exchanges
- Monitor message flows
- Delete messages
- Set up dead letter exchanges

## Building for Production

### Backend

```bash
cd rinoimob-backend

# Build JAR
mvn clean package -DskipTests

# JAR will be in target/rinoimob-backend-0.1.0-SNAPSHOT.jar

# Run JAR
java -jar target/rinoimob-backend-0.1.0-SNAPSHOT.jar
```

### Frontend App

```bash
cd rinoimob-app

# Build production bundle
npm run build

# Output in dist/
# Can be served by nginx or S3
```

### Website

```bash
cd rinoimob-website

# Build production
npm run build

# Output in .output/

# Or generate static site
npm run generate
```

## Useful Commands

### Viewing Logs

```bash
# Backend logs
docker-compose logs -f postgres

# App logs (terminal where npm run dev is running)
# Website logs (terminal where npm run dev is running)

# All Docker logs
docker-compose logs -f
```

### Stopping Services

```bash
# Stop one service
docker-compose stop postgres

# Stop all services
docker-compose down

# Stop and remove volumes (careful - deletes data)
docker-compose down -v
```

### Restarting Services

```bash
# Restart PostgreSQL
docker-compose restart postgres

# Restart all services
docker-compose restart
```

## Troubleshooting

### Port Already in Use

```bash
# Find process using port
lsof -i :8080   # Backend
lsof -i :5173   # App
lsof -i :3000   # Website

# Kill process (if needed)
kill -9 <PID>
```

### Database Connection Failed

```bash
# Verify PostgreSQL is running
docker-compose ps postgres

# Check database exists
docker exec rinoimob-postgres psql -U postgres -l | grep rinoimob

# View PostgreSQL logs
docker-compose logs postgres
```

### Redis Connection Failed

```bash
# Check Redis is running
docker-compose ps redis

# Test Redis connection
docker exec rinoimob-redis redis-cli ping

# Output should be: PONG
```

### Node Dependencies Error

```bash
# Clear cache and reinstall
npm cache clean --force
rm -rf node_modules package-lock.json
npm install
```

## Testing

### Backend Tests

```bash
cd rinoimob-backend

# Run all tests
mvn test

# Run specific test class
mvn test -Dtest=PropertyServiceTest

# Run with coverage
mvn test jacoco:report
```

### Frontend App Tests

```bash
cd rinoimob-app

# Currently no test setup - can be added
# Placeholder for future testing framework
```

### Website Tests

```bash
cd rinoimob-website

# Currently no test setup - can be added
# Placeholder for future testing framework
```

## IDE Setup

### IntelliJ IDEA

1. Open project root
2. Right-click on `pom.xml` → Add as Maven Project
3. IntelliJ will auto-index and configure
4. Set JDK 17 in Project Settings

### VS Code

1. Install extensions:
   - Java Extension Pack
   - Spring Boot Extension Pack
   - Vue - Official
   - Tailwind CSS IntelliSense
   - ESLint
   - Prettier

2. Open workspace settings and configure formatter

## Next Steps

- See [Contribution Guide](./05-CONTRIBUTION-GUIDE.md) for making changes
- Check [Coding Standards](./03-CODING-STANDARDS.md) for code style
