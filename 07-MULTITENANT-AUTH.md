# Multi-Tenant Authentication

Deep-dive into the authentication model and tenant isolation for the Rinoimob platform.

## Overview

Rinoimob uses a **shared-schema, row-level multi-tenancy** model with a **global credentials** system. A single email/password pair grants access to one or more tenant workspaces. The frontend presents a workspace selector (similar to Notion or Slack) after the user authenticates.

---

## Database Schema

### `global_credentials`

One record per email address. A single password is shared across all tenants.

```sql
CREATE TABLE global_credentials (
    email VARCHAR(255) PRIMARY KEY,
    password_hash VARCHAR(255) NOT NULL
);
```

### `users`

Membership table. One row per (email × tenant) pair.

```sql
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) NOT NULL,
    tenant_id UUID NOT NULL REFERENCES tenants(id),
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    email_verified BOOLEAN NOT NULL DEFAULT FALSE,
    verification_token VARCHAR(255),
    password_reset_token VARCHAR(255),
    password_reset_token_expiry TIMESTAMP,
    created_at TIMESTAMP NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);
```

### `tenants`

```sql
CREATE TABLE tenants (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    subdomain VARCHAR(100) UNIQUE,
    created_at TIMESTAMP NOT NULL DEFAULT NOW()
);
```

---

## Authentication Flow

### Step 1 — Identify (`POST /api/auth/identify`)

The user submits their email and password. The backend:

1. Looks up `global_credentials` by email
2. Verifies the password with bcrypt
3. Finds all `users` rows for that email (one per tenant)
4. Returns a **preAuthToken** (JWT, 5 min TTL) containing the list of allowed tenant UUIDs
5. Also returns the list of workspaces (id + name) for the frontend to display

```
Request:  { email, password }
Response: { preAuthToken, tenants: [{ id, name }] }
```

### Step 2 — Select Tenant (`POST /api/auth/select-tenant`)

The user picks a workspace. The backend:

1. Validates the `preAuthToken` (type must be `pre_auth`, not expired)
2. Checks the selected `tenantId` is in the token's `allowedTenants` claim
3. Looks up the corresponding `users` record and checks `email_verified = true`
4. Issues a full **authToken** (JWT, 24h TTL) scoped to that tenant

```
Request:  { preAuthToken, tenantId }
Response: { token, user: { id, email, firstName, lastName } }
```

### JWT Claims

| Token type | `type` claim | Extra claims |
|---|---|---|
| `preAuthToken` | `pre_auth` | `allowedTenants` (comma-separated UUIDs) |
| `authToken` | `auth` | `tenantId`, `userId` |

**JWT secret requirement**: must be ≥ 64 ASCII characters (≥ 512 bits) for HS512.

---

## Signup Flow

```
POST /api/auth/register
  → Creates global_credentials (if email is new)
  → Creates users row (linked to tenant)
  → Sends verification email with token link
  → Returns 201 (user must verify before login)

GET /api/auth/verify?token=<verificationToken>
  → Sets email_verified = true on the users row
  → Frontend redirects to login
```

---

## Password Reset Flow

```
POST /api/auth/forgot-password  { email }
  → Sets password_reset_token + expiry on ALL users rows for that email
  → Sends reset email with token link

POST /api/auth/reset-password  { token, newPassword }
  → Validates token and expiry
  → Updates global_credentials.password_hash (bcrypt)
  → Clears token from all users rows for that email
```

> Password reset is **global** by design — one password per email across all tenants.

---

## Tenant Resolution per Request

### Resolution priority (highest → lowest)

```
1. JWT authToken   → tenantId extracted by JwtAuthenticationFilter
2. X-Tenant-ID header → used by TenantInterceptor for unauthenticated routes
3. Subdomain       → parsed by TenantInterceptor from Host header
```

### Why JWT takes precedence

`JwtAuthenticationFilter` is a **Servlet Filter** — it runs before Spring MVC Interceptors. It stores the JWT-resolved tenant in `TenantContext` (ThreadLocal). `TenantInterceptor` checks `TenantContext.getTenantId() != null` at the very start of `preHandle()` and returns immediately if a tenant is already set.

This prevents an attacker from sending `X-Tenant-ID: <other-tenant-uuid>` on an authenticated request to hijack the tenant context.

```java
// TenantInterceptor.preHandle() — security guard
if (TenantContext.getTenantId() != null) {
    return true; // JWT already resolved this; do not touch
}
```

---

## Tenant Isolation at Repository Layer

Every business table has a `tenant_id` column. All repository queries must include it.

**✅ Correct pattern**

```java
// LeadRepository
Optional<Lead> findByTenantIdAndId(UUID tenantId, UUID id);
List<Lead> findByTenantIdAndAssignedTo(UUID tenantId, UUID userId);
```

**❌ Never do this**

```java
// Missing tenant filter — would return data across all tenants
List<Lead> findByAssignedTo(UUID userId);
```

Services always pass `TenantContext.getTenantId()` into repository calls:

```java
UUID tenantId = TenantContext.getTenantId();
leadRepository.findByTenantIdAndId(tenantId, leadId);
```

---

## Frontend Auth Model

### Stores

- **`useAuthStore`** — holds `token`, `user`, `isAuthenticated`; persists to `localStorage`
- **`useWorkspaceStore`** — holds `tenants` list and `selectedTenant` during the two-step flow

### Auth Guards (Vue Router)

Authenticated routes check `isAuthenticated` in `beforeEach`. Unauthenticated users are redirected to `/login`.

### AppLayout

All authenticated views use `AppLayout.vue` as a shell:

- Collapsible left sidebar with main navigation items
- Sticky header with page title slot
- User avatar (initials) + logout button in sidebar footer

### Password Strength (`usePasswordStrength`)

```typescript
const { checks, score, isValid, strengthLabel, strengthColor } = usePasswordStrength(passwordRef)
// isValid = score === 5 (all 5 checks pass)
// Used to disable submit buttons until password is strong enough
```

---

## Key Files

| File | Purpose |
|---|---|
| `JwtAuthenticationFilter.java` | Sets `TenantContext` and `SecurityContext` from JWT |
| `TenantInterceptor.java` | Fallback tenant resolution (header / subdomain) |
| `TenantContext.java` | ThreadLocal store for current request's tenant UUID |
| `AuthService.java` | identify, selectTenant, register, verify, reset, change password |
| `JwtTokenProvider.java` | Token generation and validation |
| `ValidPassword.java` / `PasswordValidator.java` | Jakarta validation constraint |
| `usePasswordStrength.ts` | Frontend composable: 5-check strength scoring |
| `AppLayout.vue` | Authenticated app shell with collapsible sidebar |
