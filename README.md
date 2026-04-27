# Rinoimob Documentation

Comprehensive documentation for the Rinoimob property management SaaS platform.

## Overview

This documentation covers architecture, development setup, coding standards, and contribution guidelines for the Rinoimob project.

## Project Status

**Phase 1 — Core Authentication & Multi-Tenancy** ✅

Implemented and tested:
- Multi-tenant architecture with per-request `TenantContext` isolation
- Global credentials model (one password per email across all tenants)
- Two-step login flow: identify → select workspace → full JWT
- Email verification on signup (SMTP configurable)
- Password reset via email token
- Strong password policy (min 6 chars, uppercase, lowercase, digit, special char)
- Collapsible left sidebar navigation (AppLayout)
- Tenant security hardening (JWT context cannot be overwritten by request headers)
- 71 backend unit tests passing

**Next**: Property management (CRUD), Leads, Negotiations modules.

## Table of Contents

1. [Setup Guide](./01-SETUP.md) - Prerequisites and initial setup
2. [Architecture](./02-ARCHITECTURE.md) - System design and technology stack
3. [Coding Standards](./03-CODING-STANDARDS.md) - Code conventions and style guides
4. [Local Development](./04-LOCAL-DEVELOPMENT.md) - Running services locally
5. [Contribution Guide](./05-CONTRIBUTION-GUIDE.md) - How to contribute
6. [Design System: Glassmorphism](./06-DESIGN-SYSTEM-GLASSMORPHISM.md) - UI/UX system
7. [Multi-Tenant Auth](./07-MULTITENANT-AUTH.md) - Auth flow and tenant isolation deep-dive

## Quick Links

- **Backend**: https://github.com/revenlab/rinoimob-backend
- **Frontend App**: https://github.com/revenlab/rinoimob-app
- **Website**: https://github.com/revenlab/rinoimob-website
- **Infrastructure**: https://github.com/revenlab/rinoimob-infrastructure

## Issues
Documentation improvements are tracked in [.projects](https://github.com/revenlab/.projects/issues?q=label%3Adocs).

## 6. Design System: Glassmorphism

Complete design system documentation for the modern Glassmorphism UI:

- **Color Palette**: Primary blues, warm oranges, and neutral tones
- **Glass Effects**: Backdrop blur, transparency, and layering
- **Component Styles**: Cards, buttons, inputs, navigation
- **Typography**: Font families, sizes, and hierarchy
- **Spacing & Borders**: Consistent sizing system
- **Animations**: Smooth transitions and hover effects
- **Vue Examples**: Complete component implementations
- **Accessibility**: WCAG compliance and contrast guidelines
- **Performance**: Best practices for mobile optimization
- **Dark Mode**: Native support for elegant dark backgrounds

👉 **[Read Full Design System](./06-DESIGN-SYSTEM-GLASSMORPHISM.md)**

## 7. Multi-Tenant Auth

Deep-dive into the authentication and multi-tenancy model:

- **Global Credentials**: one password across all tenants
- **Two-step login**: identify → workspace selector → JWT
- **Tenant isolation**: TenantContext, JWT claims, interceptor security
- **Email verification** and **password reset** flows
- **Security model**: JWT > header precedence, repository-level tenant filters

👉 **[Read Multi-Tenant Auth](./07-MULTITENANT-AUTH.md)**


## Issues
Documentation improvements are tracked in [.projects](https://github.com/revenlab/.projects/issues?q=label%3Adocs).

## 6. Design System: Glassmorphism

Complete design system documentation for the modern Glassmorphism UI:

- **Color Palette**: Primary blues, warm oranges, and neutral tones
- **Glass Effects**: Backdrop blur, transparency, and layering
- **Component Styles**: Cards, buttons, inputs, navigation
- **Typography**: Font families, sizes, and hierarchy
- **Spacing & Borders**: Consistent sizing system
- **Animations**: Smooth transitions and hover effects
- **Vue Examples**: Complete component implementations
- **Accessibility**: WCAG compliance and contrast guidelines
- **Performance**: Best practices for mobile optimization
- **Dark Mode**: Native support for elegant dark backgrounds

👉 **[Read Full Design System](./06-DESIGN-SYSTEM-GLASSMORPHISM.md)**

## 7. Multi-Tenant Auth

Deep-dive into the authentication and multi-tenancy model:

- **Global Credentials**: one password across all tenants
- **Two-step login**: identify → workspace selector → JWT
- **Tenant isolation**: TenantContext, JWT claims, interceptor security
- **Email verification** and **password reset** flows
- **Security model**: JWT > header precedence, repository-level tenant filters

👉 **[Read Multi-Tenant Auth](./07-MULTITENANT-AUTH.md)**

