# Design System: Glassmorphism

## Overview

Rinoimob uses **Glassmorphism** design pattern for all frontends (App and Website). This creates a modern, elegant UI with frosted glass effects and transparency-based layering.

## Core Principles

1. **Transparency & Blur**: Glass-like translucent panels with backdrop blur
2. **Layering**: Multiple depth layers create visual hierarchy
3. **Subtle Shadows**: Soft shadows for depth without heaviness
4. **Modern Aesthetic**: Clean, contemporary, premium feel
5. **Accessibility**: Maintain readability despite transparency

## Color Palette

### Primary Colors
```
Primary: #0066FF (Vibrant Blue)
Primary Light: #0080FF
Primary Dark: #0052CC

Secondary: #FF6B35 (Warm Orange)
Secondary Light: #FF8C5A
Secondary Dark: #E55A2B
```

### Neutral Colors
```
Dark BG: #0F1419 (Deep Navy)
Dark Card: #1A1E27 (Card Background)
Light Text: #F5F7FA (Primary Text)
Muted Text: #A0A9B8 (Secondary Text)
Border: #2A3142 (Subtle Borders)
```

### Semantic Colors
```
Success: #10B981 (Green)
Warning: #F59E0B (Amber)
Error: #EF4444 (Red)
Info: #3B82F6 (Blue)
```

## Glass Effect Components

### Card Component
```css
/* Glassmorphism Card */
.glass-card {
    background: rgba(26, 30, 39, 0.6);
    backdrop-filter: blur(20px);
    border: 1px solid rgba(255, 255, 255, 0.1);
    border-radius: 16px;
    box-shadow: 
        0 8px 32px rgba(0, 0, 0, 0.1),
        inset 0 1px 0 rgba(255, 255, 255, 0.2);
}

/* Light Glass (for contrast) */
.glass-card-light {
    background: rgba(245, 247, 250, 0.8);
    backdrop-filter: blur(20px);
    border: 1px solid rgba(0, 0, 0, 0.1);
    border-radius: 16px;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.08);
}
```

### Button Glassmorphism
```css
/* Primary Button */
.btn-primary {
    background: linear-gradient(135deg, #0066FF, #0080FF);
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.2);
    color: #F5F7FA;
    border-radius: 12px;
    padding: 12px 24px;
    font-weight: 600;
    box-shadow: 0 4px 16px rgba(0, 102, 255, 0.3);
    transition: all 0.3s ease;
}

.btn-primary:hover {
    background: linear-gradient(135deg, #0080FF, #0099FF);
    box-shadow: 0 8px 24px rgba(0, 102, 255, 0.4);
    transform: translateY(-2px);
}

/* Secondary Button */
.btn-secondary {
    background: rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.2);
    color: #F5F7FA;
    border-radius: 12px;
    padding: 12px 24px;
}

.btn-secondary:hover {
    background: rgba(255, 255, 255, 0.15);
    border-color: rgba(255, 255, 255, 0.3);
}
```

### Input Fields
```css
.input-glass {
    background: rgba(255, 255, 255, 0.05);
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.1);
    color: #F5F7FA;
    border-radius: 12px;
    padding: 12px 16px;
    transition: all 0.3s ease;
}

.input-glass:focus {
    background: rgba(255, 255, 255, 0.08);
    border-color: #0066FF;
    box-shadow: 0 0 20px rgba(0, 102, 255, 0.3);
    outline: none;
}

.input-glass::placeholder {
    color: rgba(255, 255, 255, 0.5);
}
```

## Typography

### Font Families
```css
--font-primary: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
--font-mono: 'Fira Code', 'Monaco', monospace;
```

### Font Sizes
```css
/* Heading Styles */
h1 { font-size: 3rem; font-weight: 800; letter-spacing: -0.5px; }
h2 { font-size: 2rem; font-weight: 700; letter-spacing: -0.25px; }
h3 { font-size: 1.5rem; font-weight: 600; }
h4 { font-size: 1.25rem; font-weight: 600; }

/* Body Styles */
body { font-size: 1rem; line-height: 1.6; font-weight: 400; }
small { font-size: 0.875rem; }
.caption { font-size: 0.75rem; }

/* Letter Spacing */
h1, h2, h3 { letter-spacing: -0.5px; }
.text-uppercase { letter-spacing: 1px; }
```

## Spacing System

```css
/* 8px base spacing */
--space-xs: 4px;
--space-sm: 8px;
--space-md: 16px;
--space-lg: 24px;
--space-xl: 32px;
--space-2xl: 48px;
--space-3xl: 64px;
```

## Border Radius

```css
--radius-xs: 4px;
--radius-sm: 8px;
--radius-md: 12px;
--radius-lg: 16px;
--radius-xl: 20px;
--radius-full: 9999px;
```

## Backdrop Blur Effects

### Levels of Blur
```css
/* Light Blur (subtle) */
.blur-sm { backdrop-filter: blur(4px); }

/* Medium Blur (standard glassmorphism) */
.blur-md { backdrop-filter: blur(10px); }

/* Heavy Blur (depth effect) */
.blur-lg { backdrop-filter: blur(20px); }

/* Extra Heavy Blur (modal/overlay) */
.blur-xl { backdrop-filter: blur(40px); }
```

## Shadow System

### Card Shadows
```css
/* Subtle elevation */
.shadow-sm {
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
}

/* Standard elevation */
.shadow-md {
    box-shadow: 
        0 8px 32px rgba(0, 0, 0, 0.1),
        inset 0 1px 0 rgba(255, 255, 255, 0.2);
}

/* Strong elevation */
.shadow-lg {
    box-shadow: 0 20px 48px rgba(0, 0, 0, 0.15);
}

/* Interactive elevation (hover state) */
.shadow-interactive {
    box-shadow: 0 12px 40px rgba(0, 0, 0, 0.12);
}
```

## Gradient System

### Background Gradients
```css
/* Primary gradient */
.gradient-primary {
    background: linear-gradient(135deg, #0066FF, #0080FF);
}

/* Warm gradient */
.gradient-warm {
    background: linear-gradient(135deg, #FF6B35, #FFB84D);
}

/* Cool gradient */
.gradient-cool {
    background: linear-gradient(135deg, #0066FF, #00D9FF);
}

/* Subtle dark gradient */
.gradient-dark {
    background: linear-gradient(135deg, #0F1419, #1A1E27);
}
```

## Transitions & Animations

```css
/* Smooth transitions */
.transition-smooth {
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.transition-fast {
    transition: all 0.15s ease-out;
}

.transition-slow {
    transition: all 0.5s ease-in-out;
}

/* Hover state with elevation */
.interactive:hover {
    transform: translateY(-2px);
    transition: all 0.2s ease-out;
}

/* Focus state (for accessibility) */
.interactive:focus {
    outline: 2px solid #0066FF;
    outline-offset: 2px;
}
```

## Component Examples

### Property Card
```vue
<template>
  <div class="glass-card property-card">
    <div class="card-image">
      <img :src="property.image" :alt="property.title">
      <div class="card-badge">{{ property.badge }}</div>
    </div>
    
    <div class="card-content">
      <h3 class="card-title">{{ property.title }}</h3>
      <p class="card-location">
        <i class="icon-location"></i>
        {{ property.location }}
      </p>
      
      <div class="card-features">
        <span class="feature">
          <i class="icon-bed"></i> {{ property.beds }} beds
        </span>
        <span class="feature">
          <i class="icon-bath"></i> {{ property.baths }} baths
        </span>
        <span class="feature">
          <i class="icon-area"></i> {{ property.area }} m²
        </span>
      </div>
      
      <div class="card-price">
        <span class="price">R$ {{ property.price }}</span>
        <button class="btn-primary btn-sm">View</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.glass-card {
  background: rgba(26, 30, 39, 0.6);
  backdrop-filter: blur(20px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 16px;
  overflow: hidden;
  transition: all 0.3s ease;
}

.glass-card:hover {
  background: rgba(26, 30, 39, 0.8);
  box-shadow: 0 20px 48px rgba(0, 0, 0, 0.15);
  transform: translateY(-4px);
}

.card-image {
  position: relative;
  width: 100%;
  height: 200px;
  overflow: hidden;
}

.card-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.card-badge {
  position: absolute;
  top: 12px;
  right: 12px;
  background: rgba(0, 102, 255, 0.9);
  backdrop-filter: blur(10px);
  padding: 6px 12px;
  border-radius: 20px;
  font-size: 0.75rem;
  font-weight: 600;
  color: #F5F7FA;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.card-content {
  padding: 20px;
}

.card-title {
  font-size: 1.25rem;
  font-weight: 600;
  color: #F5F7FA;
  margin-bottom: 8px;
}

.card-location {
  color: #A0A9B8;
  font-size: 0.875rem;
  margin-bottom: 16px;
  display: flex;
  align-items: center;
  gap: 6px;
}

.card-features {
  display: flex;
  gap: 16px;
  margin-bottom: 20px;
  padding-bottom: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.feature {
  font-size: 0.875rem;
  color: #A0A9B8;
  display: flex;
  align-items: center;
  gap: 6px;
}

.card-price {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.price {
  font-size: 1.5rem;
  font-weight: 700;
  color: #0066FF;
}
</style>
```

### Navigation Bar
```vue
<template>
  <nav class="glass-navbar">
    <div class="navbar-container">
      <div class="navbar-brand">
        <img src="@/assets/logo.svg" alt="Rinoimob" class="logo">
        <span class="brand-text">Rinoimob</span>
      </div>
      
      <ul class="navbar-menu">
        <li><a href="#" class="nav-link">Browse</a></li>
        <li><a href="#" class="nav-link">About</a></li>
        <li><a href="#" class="nav-link">Contact</a></li>
      </ul>
      
      <div class="navbar-actions">
        <button class="btn-secondary btn-sm">Sign In</button>
        <button class="btn-primary btn-sm">Get Started</button>
      </div>
    </div>
  </nav>
</template>

<style scoped>
.glass-navbar {
  position: sticky;
  top: 0;
  z-index: 1000;
  background: rgba(15, 20, 25, 0.5);
  backdrop-filter: blur(20px);
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.navbar-container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 16px 32px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.navbar-brand {
  display: flex;
  align-items: center;
  gap: 12px;
  font-size: 1.5rem;
  font-weight: 700;
  color: #0066FF;
}

.logo {
  width: 32px;
  height: 32px;
}

.navbar-menu {
  display: flex;
  gap: 32px;
  list-style: none;
  margin: 0;
  padding: 0;
}

.nav-link {
  color: #F5F7FA;
  text-decoration: none;
  font-weight: 500;
  position: relative;
  transition: color 0.3s ease;
}

.nav-link::after {
  content: '';
  position: absolute;
  bottom: -4px;
  left: 0;
  width: 0;
  height: 2px;
  background: #0066FF;
  transition: width 0.3s ease;
}

.nav-link:hover {
  color: #0066FF;
}

.nav-link:hover::after {
  width: 100%;
}

.navbar-actions {
  display: flex;
  gap: 12px;
}
</style>
```

### Hero Section
```vue
<template>
  <section class="hero">
    <div class="hero-background">
      <div class="bg-gradient"></div>
    </div>
    
    <div class="hero-content">
      <h1 class="hero-title">Find Your Perfect Property</h1>
      <p class="hero-subtitle">Discover amazing real estate opportunities in Portugal</p>
      
      <div class="hero-search">
        <input 
          type="text" 
          class="input-glass" 
          placeholder="Search properties..."
        >
        <button class="btn-primary">Search</button>
      </div>
    </div>
  </section>
</template>

<style scoped>
.hero {
  position: relative;
  min-height: 600px;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.hero-background {
  position: absolute;
  inset: 0;
  z-index: -1;
}

.bg-gradient {
  background: linear-gradient(135deg, #0F1419, #1A1E27);
  position: absolute;
  inset: 0;
}

.hero-content {
  text-align: center;
  z-index: 1;
  max-width: 800px;
}

.hero-title {
  font-size: 3.5rem;
  font-weight: 800;
  color: #F5F7FA;
  margin-bottom: 16px;
  letter-spacing: -1px;
}

.hero-subtitle {
  font-size: 1.25rem;
  color: #A0A9B8;
  margin-bottom: 32px;
  font-weight: 400;
}

.hero-search {
  display: flex;
  gap: 12px;
  margin-top: 32px;
}

.input-glass {
  flex: 1;
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  color: #F5F7FA;
  border-radius: 12px;
  padding: 16px 20px;
  font-size: 1rem;
  transition: all 0.3s ease;
}

.input-glass:focus {
  background: rgba(255, 255, 255, 0.08);
  border-color: #0066FF;
  box-shadow: 0 0 20px rgba(0, 102, 255, 0.3);
  outline: none;
}

.input-glass::placeholder {
  color: rgba(255, 255, 255, 0.5);
}
</style>
```

## Accessibility Guidelines

### Contrast Requirements
- Maintain WCAG AA standard contrast ratio (4.5:1 for text)
- Use solid colors for critical information
- Don't rely solely on transparency for distinction

### Focus States
```css
/* Always provide visible focus indicators */
:focus {
  outline: 2px solid #0066FF;
  outline-offset: 2px;
}

/* For keyboard navigation */
.interactive:focus-visible {
  box-shadow: 0 0 0 4px rgba(0, 102, 255, 0.2);
}
```

### Color Blindness
- Use icons in addition to colors
- Provide text labels for status indicators
- Test with color blindness simulators

## Browser Support

- Modern browsers (Chrome, Firefox, Safari, Edge)
- Requires `backdrop-filter` support
- Fallbacks:
  ```css
  .glass-card {
    background: rgba(26, 30, 39, 0.8); /* Fallback for no blur */
    @supports (backdrop-filter: blur(20px)) {
      background: rgba(26, 30, 39, 0.6);
      backdrop-filter: blur(20px);
    }
  }
  ```

## Performance Considerations

1. **Use backdrop-filter sparingly** - Can impact performance
2. **Hardware acceleration** - Use `transform: translateZ(0)` for heavy blur effects
3. **Reduce blur on mobile** - Lower blur values for better performance
4. **Lazy load images** - Especially in property cards

## Dark Mode Variants

Glassmorphism works beautifully in dark mode by default. Primary colors remain vibrant against dark backgrounds.

### Light Mode Variant (if needed)
```css
.light-theme .glass-card {
  background: rgba(245, 247, 250, 0.8);
  backdrop-filter: blur(20px);
  border: 1px solid rgba(0, 0, 0, 0.1);
  color: #0F1419;
}

.light-theme .glass-card .card-title {
  color: #0F1419;
}
```

## Resources

- Design files in: `/refs/front-and-website/`
- TailwindCSS configuration for Glassmorphism
- Figma components library (to be created)
- Storybook documentation (Phase 3)

## Implementation Checklist

- [ ] Install TailwindCSS with backdrop blur support
- [ ] Create CSS module with glass effect utilities
- [ ] Create reusable component library
- [ ] Test contrast and accessibility
- [ ] Test on various devices and browsers
- [ ] Document in Storybook

