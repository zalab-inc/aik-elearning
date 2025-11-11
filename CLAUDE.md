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

# Database commands
pnpm db:generate    # Generate Prisma Client
pnpm db:push        # Push schema changes to database (development)
pnpm db:migrate     # Create and run migrations
pnpm db:studio      # Open Prisma Studio GUI
```

Note: This project uses `pnpm` as the package manager (evidenced by `pnpm-lock.yaml`).

## Architecture

### Tech Stack
- **Framework**: Next.js 16.0.1 with App Router
- **React**: v19.2.0 with React Compiler enabled (configured in `next.config.ts`)
- **Database**: Prisma 6.19.0 with MySQL
- **Styling**: Tailwind CSS v4 with custom theme configuration
- **Component Library**: shadcn/ui (New York style variant with lucide-react icons)
- **TypeScript**: Strict mode enabled
- **CSS Variables**: Using OKLCH color space for theme colors

### Project Structure
```
src/
├── app/              # Next.js App Router pages and layouts
│   ├── auth/         # Authentication routes
│   │   └── login/    # Login feature (page, form, actions, schemas)
│   ├── layout.tsx    # Root layout with Geist fonts
│   ├── page.tsx      # Home page
│   └── globals.css   # Global styles with Tailwind v4 and theme variables
├── lib/
│   ├── prisma.ts     # Prisma Client singleton instance
│   └── utils.ts      # Utility functions (cn helper for className merging)
└── generated/
    └── prisma/       # Generated Prisma Client (auto-generated, do not edit)

prisma/
├── schema.prisma     # Database schema definition
└── migrations/       # Database migrations (created by `db:migrate`)
```

**Feature Organization**: Authentication features follow a modular pattern where each feature directory contains:
- `page.tsx` - Next.js page component
- `*-form.tsx` - Client-side form components (marked with "use client")
- `*-actions.ts` - Server actions (marked with "use server")
- `*-schemas.ts` - Validation schemas

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

**Prisma Configuration**:
- Database: MySQL at `localhost:3306/aik-elearning` (credentials: root/root)
- Prisma Client generated to: `src/generated/prisma/`
- Schema location: `prisma/schema.prisma`
- Config file: `prisma.config.ts` (loads environment variables via dotenv)
- Environment variables: Defined in `.env` file (gitignored)

### Development Patterns

**UI Components**:
- shadcn/ui components should be placed in `src/components/ui/`
- Use the `cn()` utility function from `src/lib/utils.ts` for merging Tailwind classes
- Dark mode is implemented using CSS class-based approach (`.dark` class)

**Database Access**:
- Always import Prisma Client from `@/lib/prisma` (singleton instance)
- Use Prisma Client in Server Components and Server Actions only
- Never import Prisma Client in Client Components (marked with "use client")
- Run `pnpm db:generate` after modifying `prisma/schema.prisma`
- Use `pnpm db:push` for schema changes during development
- Use `pnpm db:migrate` to create migration files for production

**Server Actions Pattern**:
```typescript
// Example: src/app/auth/login/login-actions.ts
"use server"

import prisma from "@/lib/prisma"

export async function loginAction(formData: FormData) {
  const user = await prisma.user.findUnique({
    where: { email: formData.get("email") as string }
  })
  // ... authentication logic
}
```
