# Coding Standards

Code conventions and style guides for the Rinoimob project.

## Code Style

### K&R Style (All Languages)

All code follows **K&R style** formatting:
- Opening braces on the same line as the statement
- Closing braces on their own line
- 4-space indentation
- No tabs

#### Java Example
```java
public class UserService {
    public User createUser(String email) {
        if (email == null || email.isEmpty()) {
            throw new IllegalArgumentException("Email required");
        }
        User user = new User(email);
        return userRepository.save(user);
    }
}
```

#### TypeScript Example
```typescript
export class UserStore {
    async createUser(email: string): Promise<User> {
        if (!email) {
            throw new Error("Email required");
        }
        const user = new User(email);
        return await this.userApi.save(user);
    }
}
```

#### SQL Example
```sql
CREATE TABLE users (
    id BIGINT PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
    guid UUID NOT NULL UNIQUE DEFAULT gen_random_uuid(),
    email VARCHAR(255) NOT NULL UNIQUE,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    deleted_at TIMESTAMP
);
```

## Java Backend

### File Structure
```
src/main/java/com/rinoimob/
├── api/
│   ├── controller/
│   ├── dto/
│   │   ├── request/
│   │   └── response/
│   └── exception/
├── domain/
│   └── entity/
├── service/
├── repository/
├── config/
├── security/
└── util/
```

### Naming Conventions
- **Classes**: PascalCase (`UserService`, `PropertyController`)
- **Methods**: camelCase (`createUser()`, `findPropertiesByTenantId()`)
- **Constants**: UPPER_SNAKE_CASE (`MAX_PAGE_SIZE`, `DEFAULT_LOCALE`)
- **Variables**: camelCase (`userId`, `tenantContext`)
- **Packages**: lowercase with dots (`com.rinoimob.domain`, `com.rinoimob.service`)

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
├── components/
│   ├── common/
│   ├── forms/
│   └── layout/
├── views/
├── stores/
├── services/
├── types/
├── utils/
├── styles/
└── assets/
```

### Naming Conventions
- **Classes**: PascalCase (`UserStore`, `PropertyApi`)
- **Functions**: camelCase (`createUser()`, `fetchProperties()`)
- **Constants**: UPPER_SNAKE_CASE (`API_BASE_URL`, `MAX_RETRIES`)
- **Variables**: camelCase (`userId`, `tenantId`)
- **Components**: PascalCase (`UserForm.vue`, `PropertyCard.vue`)
- **Stores**: camelCase (`useUserStore`, `usePropertyStore`)

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

## Git Conventions

### Branch Naming

Branches follow the pattern: **issueNUMBER**

Examples:
- `issue1` - Multi-Tenant Architecture
- `issue9` - User Registration with Email Validation
- `issue23` - Property CRUD Operations
- `issue494` - Any future issue

### Commit Message Format

All commits must follow this format:

```
issueNUMBER: Brief description of changes

Detailed explanation of changes made:
- Bullet point 1
- Bullet point 2
- Bullet point 3

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

#### Example
```
issue1: Implement multi-tenant architecture

- Add TenantContext middleware for request filtering
- Configure row-level security (RLS) policies
- Add audit logging (actor_id, tenant_id, ip_address)
- Implement tenant_id injection into all queries
- Add unit tests for tenant isolation

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

## API Documentation

### Swagger/OpenAPI Annotations

All API endpoints must be documented with Swagger annotations:

```java
@RestController
@RequestMapping("/api/v1/users")
@OpenAPIDefinition(info = @Info(title = "User API", version = "1.0"))
public class UserController {

    @PostMapping
    @Operation(summary = "Create a new user", description = "Register a new user with email validation")
    @ApiResponses(value = {
        @ApiResponse(responseCode = "201", description = "User created successfully"),
        @ApiResponse(responseCode = "400", description = "Invalid input"),
        @ApiResponse(responseCode = "409", description = "Email already exists")
    })
    public ResponseEntity<UserResponse> createUser(
        @Valid @RequestBody CreateUserRequest request) {
        return ResponseEntity.status(201).body(userService.createUser(request));
    }
}
```

### DTO Documentation

```java
@Data
@Schema(description = "User registration request")
public class CreateUserRequest {
    
    @Schema(description = "User email address", example = "user@example.com")
    @Email
    private String email;
    
    @Schema(description = "Password (min 10 chars, uppercase, lowercase, number, symbol)")
    @NotBlank
    private String password;
}
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

## Testing Standards

### Test Naming
- Unit tests: `{ClassName}Test.java`
- Integration tests: `{ClassName}IntegrationTest.java`
- Test methods: `should{ExpectedBehavior}When{Condition}()`

### Example
```java
class UserServiceTest {
    
    @Test
    void shouldCreateUserWhenValidEmailProvided() {
        // Arrange
        String email = "test@example.com";
        
        // Act
        User result = userService.createUser(email);
        
        // Assert
        assertThat(result.getEmail()).isEqualTo(email);
    }
    
    @Test
    void shouldThrowExceptionWhenEmailIsNull() {
        // Arrange
        String email = null;
        
        // Act & Assert
        assertThatThrownBy(() -> userService.createUser(email))
            .isInstanceOf(IllegalArgumentException.class);
    }
}
```

## Security Standards

### Password Hashing
- Use **Argon2id** for password hashing
- Never store plaintext passwords
- Use Spring Security's `PasswordEncoder` abstraction

### Authentication
- Use **JWT (JSON Web Tokens)** for stateless auth
- Include tenant_id in token claims
- Set appropriate token expiration (15 min access, 7 day refresh)

### Authorization
- Implement row-level security at database level
- Always filter queries by tenant_id
- Use Spring Security annotations (@PreAuthorize, @Secured)

### Audit Logging
- Log all user actions with: actor_id, tenant_id, action, timestamp, ip_address
- Implement soft deletes (deleted_at timestamp, never hard delete)

## Comments

Only comment code that needs clarification - comments should explain **why**, not **what**:

```java
// Good
// Calculate additional charges for late payments
double lateCharge = amount * 0.05;

// Bad  
// Multiply amount by 0.05
double lateCharge = amount * 0.05;
```

## Quality Checklist

Before submitting a PR:
- [ ] Follows K&R style
- [ ] Naming conventions matched
- [ ] Tests written and passing
- [ ] No hardcoded values (use environment variables)
- [ ] Swagger annotations added (backend)
- [ ] Comments added for complex logic
- [ ] No console.log or System.out.println left
- [ ] Commit message follows format
- [ ] Branch name matches issueNUMBER pattern

## Frontend Design System: Glassmorphism

All frontends (App and Website) use the **Glassmorphism** design pattern.

### Core Elements

**Glass Cards**
```vue
<div class="glass-card">
  <div class="card-content">
    <!-- Content -->
  </div>
</div>

<style scoped>
.glass-card {
  background: rgba(26, 30, 39, 0.6);
  backdrop-filter: blur(20px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 16px;
  box-shadow: 
    0 8px 32px rgba(0, 0, 0, 0.1),
    inset 0 1px 0 rgba(255, 255, 255, 0.2);
}
</style>
```

**Color Palette**
```
Primary: #0066FF (Vibrant Blue)
Secondary: #FF6B35 (Warm Orange)
Dark BG: #0F1419 (Deep Navy)
Dark Card: #1A1E27
Light Text: #F5F7FA
Muted Text: #A0A9B8
```

**Blur Effects**
```css
/* Light blur (subtle) */
backdrop-filter: blur(4px);

/* Medium blur (standard) */
backdrop-filter: blur(10px);

/* Heavy blur (depth) */
backdrop-filter: blur(20px);
```

### Design Standards

- Use K&R style CSS (opening braces on same line)
- Follow glassmorphism color palette
- Apply backdrop blur to all cards and overlays
- Maintain consistent spacing (8px base unit)
- Use soft shadows for depth
- Ensure WCAG AA contrast ratios (4.5:1)
- Test on dark backgrounds (primary theme)

### Complete Design System

See **[Design System: Glassmorphism](./06-DESIGN-SYSTEM-GLASSMORPHISM.md)** for:
- Full color palette and gradients
- Component examples (Property Card, Navigation, Hero)
- Typography system
- Animation guidelines
- Accessibility requirements
- Performance best practices

