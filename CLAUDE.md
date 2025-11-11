# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an e-learning platform built with Next.js 16, React 19, and TypeScript. The project uses the Next.js App Router architecture and is configured with shadcn/ui component library integration.

## Development Commands

```bash
# Start development server (http://localhost:3000)
pnpm dev

# Build for production
pnpm build

# Start production server
pnpm start

# Run linting
pnpm lint
```

Note: This project uses `pnpm` as the package manager (evidenced by `pnpm-lock.yaml`).

## Architecture

### Tech Stack
- **Framework**: Next.js 16.0.1 with App Router
- **React**: v19.2.0 with React Compiler enabled (configured in `next.config.ts`)
- **Styling**: Tailwind CSS v4 with custom theme configuration
- **Component Library**: shadcn/ui (New York style variant with lucide-react icons)
- **TypeScript**: Strict mode enabled
- **CSS Variables**: Using OKLCH color space for theme colors

### Project Structure
```
src/
├── app/              # Next.js App Router pages and layouts
│   ├── layout.tsx    # Root layout with Geist fonts
│   ├── page.tsx      # Home page
│   └── globals.css   # Global styles with Tailwind v4 and theme variables
└── lib/
    └── utils.ts      # Utility functions (cn helper for className merging)
```

### Path Aliases
Configured in `tsconfig.json`:
- `@/*` → `./src/*`

shadcn/ui component aliases (configured in `components.json`):
- `@/components` → Components directory
- `@/components/ui` → UI components
- `@/lib` → Library/utility functions
- `@/hooks` → Custom React hooks
- `@/lib/utils` → Utility functions (includes `cn` helper)

### Key Configuration Details

**React Compiler**: Enabled in `next.config.ts` (`reactCompiler: true`). This experimental feature optimizes React components automatically.

**Tailwind Configuration**:
- Uses Tailwind CSS v4 with `@tailwindcss/postcss`
- Custom theme inline configuration in `globals.css` using CSS variables
- Dark mode variant defined as `@custom-variant dark (&:is(.dark *))`
- Includes `tw-animate-css` for animation utilities
- Color scheme uses OKLCH color space for better perceptual uniformity
- Theme includes sidebar, chart, and standard UI colors with full dark mode support

**shadcn/ui Setup**:
- Style: `new-york`
- RSC (React Server Components): enabled
- Base color: `neutral`
- Icon library: `lucide-react`
- Components should be added to `@/components/ui`

**TypeScript Configuration**:
- Strict mode enabled
- Target: ES2017
- Module resolution: bundler
- JSX: react-jsx (for React 19)

**Fonts**:
- Uses Geist Sans and Geist Mono from `next/font/google`
- Font variables: `--font-geist-sans` and `--font-geist-mono`

### Development Patterns

When adding shadcn/ui components, they should be placed in `src/components/ui/` and follow the configured aliases.

The `cn()` utility function in `src/lib/utils.ts` should be used for merging Tailwind classes and handling conditional styling.

Dark mode is implemented using CSS class-based approach (`.dark` class) with comprehensive theme variable support.
