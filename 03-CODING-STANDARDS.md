# Coding Standards

Code conventions and style guides for the Rinoimob project.

## General Principles

- **Readability**: Code should be easy to understand
- **Consistency**: Follow established patterns within the codebase
- **Maintainability**: Write code that's easy to modify and test
- **Security**: Follow security best practices
- **Performance**: Write efficient code

## Java Backend

### File Structure
```
src/main/java/com/rinoimob/
├── api/                 # REST Controllers
├── domain/             # JPA Entities and DTOs
├── service/            # Business logic services
├── repository/         # Data access layer
├── config/             # Spring configuration
├── exception/          # Custom exceptions
├── util/               # Utility classes
└── RinoimobApplication.java
```

### Naming Conventions
- **Classes**: PascalCase (`UserService`, `PropertyController`)
- **Methods**: camelCase (`getPropertyById`, `createUser`)
- **Constants**: UPPER_SNAKE_CASE (`MAX_RETRY_ATTEMPTS`, `API_VERSION`)
- **Variables**: camelCase (`userId`, `propertyName`)

### Code Style
- **Indentation**: 4 spaces
- **Line Length**: Maximum 120 characters
- **Brackets**: Allman style for multi-line blocks
```java
public void processProperty(Property property)
{
    if (property != null)
    {
        // Handle property
    }
}
```

### Annotations
- Use `@Slf4j` from Lombok for logging
- Use `@Autowired` for dependency injection
- Use `@Transactional` for database operations
- Document public APIs with Javadoc

### Testing
```java
@SpringBootTest
class PropertyServiceTest
{
    private PropertyService service;
    
    @Test
    void shouldFindPropertyById()
    {
        // Arrange
        Long propertyId = 1L;
        
        // Act
        Property result = service.findById(propertyId);
        
        // Assert
        assertThat(result).isNotNull();
    }
}
```

## TypeScript / Vue / Nuxt

### File Structure
```
src/
├── components/    # Reusable Vue components
├── views/        # Page components
├── stores/       # Pinia state management
├── router/       # Route configuration
├── types/        # TypeScript interfaces
├── utils/        # Helper functions
├── services/     # API services
└── styles/       # CSS/SCSS files
```

### Naming Conventions
- **Components**: PascalCase (`UserCard.vue`, `PropertyForm.vue`)
- **Functions**: camelCase (`formatDate`, `calculateTotal`)
- **Constants**: UPPER_SNAKE_CASE (`API_TIMEOUT`, `MAX_RETRIES`)
- **Variables**: camelCase (`userId`, `isLoading`)

### TypeScript Style
```typescript
// Always use interfaces for props
interface User {
  id: string
  email: string
  name: string
}

// Use type annotations
const getUserById = (id: string): Promise<User> => {
  // Implementation
}

// Avoid `any`, use specific types
const processData = (data: Record<string, unknown>): void => {
  // Implementation
}
```

### Vue Component Style
```vue
<template>
  <div class="component">
    <h2>{{ title }}</h2>
    <button @click="handleClick">Click me</button>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

interface Props {
  title: string
}

const props = withDefaults(defineProps<Props>(), {
  title: 'Default Title'
})

const emit = defineEmits<{
  click: [value: string]
}>()

const count = ref(0)
const handleClick = () => {
  count.value++
  emit('click', `Clicked ${count.value} times`)
}
</script>

<style scoped>
.component {
  padding: 1rem;
}
</style>
```

### Tailwind CSS
- Use utility classes for styling
- Avoid inline styles
- Group related classes
```vue
<div class="flex flex-col gap-4 rounded-lg border border-gray-200 p-6 shadow-sm">
  <!-- Content -->
</div>
```

## Git Commits

### Commit Message Format

```
<type>(<scope>): <subject>

<body>

<footer>

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

### Types
- `feat`: A new feature
- `fix`: A bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, missing semicolons, etc.)
- `refactor`: Code refactoring
- `perf`: Performance improvements
- `test`: Adding or updating tests
- `chore`: Build, CI/CD, dependencies

### Example Commits
```
feat(auth): implement JWT token refresh

Add automatic token refresh mechanism to maintain user sessions.
Tokens are refreshed when they expire within the next hour.

- Add RefreshTokenService
- Add @RefreshToken annotation
- Add integration tests

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

```
fix(property): handle null description in form

The property form was crashing when description was null.
This adds proper null checking.

Fixes #123

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

## Pull Request Guidelines

### Branch Naming
- `feature/description` - New features
- `bugfix/description` - Bug fixes
- `hotfix/description` - Critical production fixes
- `refactor/description` - Code refactoring

### PR Template
```markdown
## Description
Brief description of changes

## Related Issue
Fixes #(issue number)

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change

## Testing
How were these changes tested?

## Checklist
- [ ] My code follows the style guidelines
- [ ] I have performed a self-review
- [ ] I have commented complex areas
- [ ] Tests added/updated
- [ ] Documentation updated
```

## Code Review Checklist

- [ ] Code follows style guidelines
- [ ] Tests are included and passing
- [ ] Documentation is updated
- [ ] No unnecessary dependencies added
- [ ] Security best practices followed
- [ ] Performance implications considered
- [ ] Error handling is comprehensive

## Tools

### Java
- **Formatter**: Spring Framework conventions
- **Linter**: Checkstyle
- **Code Analysis**: SonarQube (future)

### TypeScript/Vue
- **Formatter**: Prettier (configured in project)
- **Linter**: ESLint
- **Type Checking**: TypeScript strict mode

## Comments

Comments should explain **why**, not **what**:

```java
// Good
// Calculate additional charges for late payments
double lateCharge = amount * 0.05;

// Bad
// Multiply amount by 0.05
double lateCharge = amount * 0.05;
```

## Documentation

- README files for each component
- Inline comments for complex logic
- JSDoc/Javadoc for public APIs
- Architecture Decision Records (ADR) for major changes
