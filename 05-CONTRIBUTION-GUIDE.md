# Contribution Guide

How to contribute to the Rinoimob project.

## Getting Started

1. Fork the repository (if external contributor)
2. Clone your fork or the main repository
3. Create a feature branch
4. Make your changes
5. Submit a pull request

## Branch Naming Convention

All branches must follow this naming convention:

### Feature Branches
```
feature/<ticket-id>-<short-description>
feature/RINO-123-user-authentication
feature/auth-jwt-implementation
```

### Bugfix Branches
```
bugfix/<ticket-id>-<short-description>
bugfix/RINO-456-null-pointer-exception
bugfix/property-form-crash
```

### Hotfix Branches (Production Fixes)
```
hotfix/<ticket-id>-<short-description>
hotfix/RINO-789-database-connection-leak
hotfix/security-vulnerability-patch
```

### Refactor Branches
```
refactor/<short-description>
refactor/auth-service-consolidation
refactor/api-response-wrapper
```

## Git Commit Format

Always use the conventional commit format with Copilot co-author:

```
<type>(<scope>): <subject>

<body>

<footer>

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

### Types

- **feat**: A new feature
- **fix**: A bug fix
- **docs**: Documentation changes
- **style**: Code style changes (formatting, missing semicolons, etc.)
- **refactor**: Code refactoring
- **perf**: Performance improvements
- **test**: Adding or updating tests
- **chore**: Build, CI/CD, dependencies

### Scope

The scope should specify which part of the codebase:
- `auth`: Authentication related
- `property`: Property management
- `user`: User management
- `api`: API layer
- `ui`: User interface
- `config`: Configuration
- `test`: Testing

### Subject

- Use imperative mood ("add" not "added" or "adds")
- Don't capitalize first letter
- No period (.) at the end
- Max 50 characters

### Body

- Explain **what** and **why**, not **how**
- Wrap at 72 characters
- Separate from subject with blank line
- Use bullet points for multiple changes

### Footer

- Reference issues: `Fixes #123`, `Closes #456`
- Document breaking changes: `BREAKING CHANGE: description`

### Example Commits

```
feat(auth): implement JWT token refresh

Add automatic token refresh mechanism to maintain user sessions
without requiring re-login. Tokens are refreshed when they expire
within the next hour.

- Add RefreshTokenService with scheduled refresh logic
- Add @RefreshToken annotation for protected endpoints
- Add integration tests for token lifecycle
- Update SecurityConfig to register token refresh filter

Fixes #123

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

```
fix(property): handle null description in form

The property form was crashing when the description field
was null. This adds proper null checking and validation.

- Add null check in PropertyFormComponent
- Add test case for null description
- Update PropertyValidator to handle edge cases

Fixes #456

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

```
refactor(api): consolidate response handling

Consolidate HTTP response handling into a single ResponseWrapper
to provide consistent response format across all endpoints.

- Create ResponseWrapper<T> class
- Update all controllers to use wrapper
- Remove duplicate response formatting code
- Add tests for response wrapper

BREAKING CHANGE: API responses now wrapped in ResponseWrapper.
Update clients to access data from response.data field.

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

## Pull Request Process

### Before Creating PR

1. **Update from main**
   ```bash
   git fetch origin
   git rebase origin/main
   ```

2. **Run tests**
   ```bash
   # Backend
   cd rinoimob-backend && mvn test
   
   # App
   cd rinoimob-app && npm run build
   
   # Website
   cd rinoimob-website && npm run build
   ```

3. **Build verification**
   ```bash
   # Backend
   mvn clean package -DskipTests
   
   # App
   npm run build
   
   # Website
   npm run build
   ```

### Creating PR

1. Push your branch
   ```bash
   git push origin feature/RINO-123-your-feature
   ```

2. Go to GitHub and create Pull Request
3. Fill out the PR template completely

### PR Template

```markdown
## Description
Brief description of what this PR does

## Related Issue
Fixes #(issue number)
Relates to #(issue number)

## Type of Change
- [ ] Bug fix (non-breaking)
- [ ] New feature (non-breaking)
- [ ] Breaking change
- [ ] Documentation update

## Changes Made
- Change 1
- Change 2
- Change 3

## Testing
How were these changes tested?
- [ ] Unit tests added
- [ ] Integration tests added
- [ ] Manual testing completed

## Checklist
- [ ] Code follows project style guidelines
- [ ] Self-review of code completed
- [ ] Comments added for complex logic
- [ ] Documentation updated
- [ ] Tests added/updated
- [ ] Build passes
- [ ] No new warnings

## Screenshots (if applicable)
Add screenshots for UI changes
```

## Code Review Process

### Review Guidelines

Reviewers will check:
- ✅ Code follows style guidelines (see [Coding Standards](./03-CODING-STANDARDS.md))
- ✅ Tests are included and passing
- ✅ Documentation is updated
- ✅ No unnecessary dependencies added
- ✅ Security best practices followed
- ✅ Performance implications considered
- ✅ Error handling is comprehensive

### Responding to Reviews

1. Address all comments
2. Push additional commits for changes
3. Re-request review after making changes
4. Avoid force-pushing (it deletes conversation history)

## Common Issues

### Merge Conflicts

```bash
# Update with latest main
git fetch origin
git rebase origin/main

# Resolve conflicts in your editor
# Then continue
git rebase --continue

# Force push your branch
git push origin feature/branch-name --force-with-lease
```

### Accidental Commits on Wrong Branch

```bash
# Create new branch from current position
git branch feature/correct-branch

# Reset current branch to before the commits
git reset origin/main

# Switch to new branch
git checkout feature/correct-branch
```

## Continuous Integration

All pull requests trigger automated checks:

- ✅ Java build and tests (Maven)
- ✅ JavaScript build and type checking (TypeScript)
- ✅ Docker Compose validation
- ✅ Code quality checks (future)

All checks must pass before merging.

## Release Process

When ready for release:

1. Create release branch: `release/v0.2.0`
2. Update version numbers
3. Update CHANGELOG
4. Create release notes
5. Tag release: `v0.2.0`
6. Merge back to main and develop

## Support

- 📧 Email: support@rinoimob.com
- 💬 Discussions: GitHub Discussions
- 🐛 Issues: GitHub Issues

## Code of Conduct

- Be respectful and inclusive
- Provide constructive feedback
- Help others learn and grow
- Follow project values

Thank you for contributing to Rinoimob! 🎉
