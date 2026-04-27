# Contribution Guide

How to contribute to the Rinoimob project.

## Branch Naming Convention

All feature branches must follow the pattern: **issueNUMBER**

Examples:
```
issue1      (Multi-Tenant Architecture)
issue9      (User Registration)
issue23     (Property CRUD)
issue494    (Any future issue)
```

### Creating a Branch

```bash
git checkout -b issue1
```

**Never use branch names like:**
- `feature/something`
- `bugfix/something`
- `my-feature`
- `temp-branch`

Always use the issue number as the branch name.

## Commit Message Format

Every commit must follow this format:

```
issueNUMBER: Brief description of changes

Detailed explanation:
- Change 1
- Change 2
- Change 3

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

### Example Commit

```bash
git commit -m "issue1: Implement multi-tenant architecture

- Add TenantContext middleware for request filtering
- Configure row-level security (RLS) policies
- Add audit logging (actor_id, tenant_id, ip_address)
- Implement tenant_id injection into all queries
- Add unit tests for tenant isolation

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>"
```

### Commit Best Practices

- **Keep commits small and focused** - one issue per commit
- **Write descriptive messages** - future developers need to understand why
- **Include the Copilot co-author trailer** - always on every commit
- **Use imperative mood** - "Add feature" not "Added feature"

## Pull Request Workflow

### 1. Create Feature Branch
```bash
git checkout -b issue9
```

### 2. Make Changes
- Follow coding standards (K&R style, naming conventions)
- Write tests for new code
- Update documentation
- Add Swagger annotations (backend APIs)

### 3. Commit Changes
```bash
git add .
git commit -m "issue9: User registration implementation

- Implement registration endpoint with email validation
- Add Argon2id password hashing
- Create email verification flow
- Add unit and integration tests
- Update API documentation

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>"
```

### 4. Push to GitHub
```bash
git push origin issue9
```

### 5. Open Pull Request

**PR Title Format:**
```
[issue9] User registration with email validation
```

**PR Description Template:**
```markdown
## Issue
Closes #9

## Changes
- Describe change 1
- Describe change 2
- Describe change 3

## Testing
- How was this tested?
- Test results

## Screenshots (if applicable)
- Before/after images

## Checklist
- [ ] Code follows K&R style
- [ ] Tests written and passing
- [ ] API documentation updated (if backend)
- [ ] No hardcoded values
- [ ] Database schema documented (if changed)
- [ ] No console.log or System.out.println
```

### 6. Code Review

- At least one approval required before merge
- Address all feedback before merging
- Resolve conflicts if any
- Ensure all CI/CD checks pass

### 7. Merge to Main

Once approved:
```bash
# Option 1: Squash commits (preferred for clean history)
git merge --squash issue9

# Option 2: Merge with history
git merge issue9
```

Then delete the branch:
```bash
git branch -d issue9
git push origin --delete issue9
```

## Code Review Checklist

Reviewers should verify:

### Code Quality
- [ ] Follows K&R style
- [ ] Naming conventions matched
- [ ] No duplication
- [ ] Comments explain "why" not "what"
- [ ] No hardcoded values

### Testing
- [ ] Unit tests written
- [ ] Integration tests for critical paths
- [ ] Tests are passing
- [ ] Coverage adequate

### Documentation
- [ ] README updated (if needed)
- [ ] API docs updated (if backend)
- [ ] Database schema documented (if changed)
- [ ] Comments added for complex logic

### Security
- [ ] No sensitive data exposed
- [ ] Input validation implemented
- [ ] SQL injection prevention
- [ ] XSS prevention (frontend)
- [ ] CSRF protection (frontend)
- [ ] Authentication/authorization checks

### Performance
- [ ] No N+1 queries
- [ ] Efficient algorithms
- [ ] No memory leaks
- [ ] Appropriate caching used

## Development Workflow

### Backend Development

1. Create branch: `git checkout -b issue1`
2. Update `pom.xml` for dependencies
3. Create entity classes
4. Create repository interfaces
5. Create service classes with business logic
6. Create controller endpoints with Swagger annotations
7. Write unit tests
8. Write integration tests
9. Commit and push
10. Create PR with checklist

### Frontend Development

1. Create branch: `git checkout -b issue23`
2. Create components
3. Update stores (Pinia)
4. Add forms and validation
5. Add error handling
6. Add loading states
7. Write component tests
8. Commit and push
9. Create PR with checklist

### Documentation

1. Create branch: `git checkout -b issue-docs-1`
2. Update markdown files
3. Add examples
4. Verify formatting
5. Commit and push
6. Create PR

## Common Commands

### Git Commands
```bash
git checkout -b issue9        # Create issue branch
git status                    # Check status
git add .                     # Stage changes
git commit -m "issue9: ..."   # Commit
git push origin issue9        # Push to GitHub
```

### Maven Commands
```bash
mvn clean              # Clean build
mvn compile            # Compile code
mvn test               # Run tests
mvn package            # Create JAR
mvn spring-boot:run    # Run application
```

### npm Commands
```bash
npm install            # Install dependencies
npm run dev            # Start dev server
npm run build          # Build for production
npm run test           # Run tests
npm run lint           # Run linter
```

### Docker Commands
```bash
docker-compose up -d                # Start services
docker-compose down                 # Stop services
docker-compose logs -f              # View logs
docker-compose ps                   # View status
docker-compose restart postgres     # Restart service
```

## Troubleshooting

### Port Already in Use

```bash
lsof -i :39000
lsof -i :5173
lsof -i :3000
```

### Database Connection Error

```bash
docker-compose ps postgres
docker-compose logs postgres
docker-compose restart postgres
```

### Node Modules Issues

```bash
npm cache clean --force
rm -rf node_modules package-lock.json
npm install
```

### Maven Build Failure

```bash
rm -rf ~/.m2/repository
mvn clean install
```

## Performance Tips

1. Use volume mounts for source code
2. Enable Docker BuildKit
3. Cache npm/Maven dependencies
4. Use .dockerignore
5. Run tests in CI/CD

## Next Steps

- Review 03-CODING-STANDARDS.md for code conventions
- Review 05-CONTRIBUTION-GUIDE.md for PR workflow
- Check out existing PRs for examples

## Questions?

- Review the documentation in this folder
- Check existing PRs for examples
- Ask in team discussions
- Reference the coding standards guide
