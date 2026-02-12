---
name: app
description: >
  Main entry point for Ozzy (OSSIPA) development - quick reference for all components.
  Trigger: General Ozzy development questions, project overview, component navigation.
license: Apache-2.0
metadata:
  author: Vinco Orbis Team
  version: "1.0"
  scope: [root]
  auto_invoke: "General Ozzy development questions"
allowed-tools: Read, Edit, Write, Glob, Grep, Bash, WebFetch, WebSearch, Task
---

## Project Overview

**Ozzy (OSSIPA)** is a project management application built with modern web technologies.

## Tech Stack

| Technology | Version | Purpose |
|------------|---------|---------|
| TypeScript | 5.9.3 | Language |
| React | 18.2.0 | Framework |
| Vite | 7.2.4 | Build Tool/Compiler |
| Tailwind CSS | 3.4.18 | Styling |
| React Router | 7.9.6 | Routing |
| Axios | 1.13.2 | HTTP Client |
| React Query | 5.90.11 | Cache/Server State |
| Zustand | 5.0.9 | Client State Manager |
| Formik | 2.4.9 | Forms |
| Yup | 1.7.1 | Validation |
| Sonner | 2.0.7 | Toast Notifications |
| React-Leaflet | 4.2.1 | Maps |

## Quick Commands

```bash
# Development
npm run dev              # Start dev server at localhost:5173

# Build
npm run build           # TypeScript check + production build

# Code Quality
npm run lint            # Run ESLint
npm run format          # Format code with Prettier

# Preview
npm run preview         # Preview production build
```

## Project Structure

```
ozzy-agents/
├── src/                # Source code
├── public/             # Static assets
├── docs/               # Documentation
├── context/            # AI agent context & skills
├── dist/               # Build output
└── node_modules/       # Dependencies
```

## Commit Style

`feat:`, `fix:`, `docs:`, `chore:`, `perf:`, `refactor:`, `test:`

## Related Skills

- `app-ui` - React/Vite patterns
- `typescript` - TypeScript best practices
- `react-19` - React patterns (compatible with 18.2)
- `tailwind-4` - Tailwind CSS patterns (compatible with 3.4)
- `zustand-5` - Zustand state management
- `yup-1` - Yup validation schemas

## Resources

- **Setup**: See [SETUP_LOCAL.md](../../SETUP_LOCAL.md) for local environment setup
- **Contributing**: See [docs/CONTRIBUTING.md](../../docs/CONTRIBUTING.md) for contribution guidelines
- **Team**: Vinco Orbis development team
