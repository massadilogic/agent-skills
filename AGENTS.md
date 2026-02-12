# Repository Guidelines

## How to Use This Guide

- Start here for cross-project norms. This repository is a monorepo with several components.
- Each component has an `AGENTS.md` file with specific guidelines (e.g., `api/AGENTS.md`, `ui/AGENTS.md`).
- Component docs override this file when guidance conflicts.

## Available Skills

Use these skills for detailed patterns on-demand:

### Generic Skills (Any Project)
| Skill | Description | URL |
|-------|-------------|-----|
| `typescript` | Const types, flat interfaces, utility types | [SKILL.md](context/skills/typescript/SKILL.md) |
| `react-19` | No useMemo/useCallback, React Compiler | [SKILL.md](context/skills/react-19/SKILL.md) |
| `tailwind-4` | cn() utility, no var() in className | [SKILL.md](context/skills/tailwind-4/SKILL.md) |
| `zustand-5` | Persist, selectors, slices | [SKILL.md](context/skills/zustand-5/SKILL.md) |
| `yup-1` | Yup schema validation patterns | [SKILL.md](context/skills/yup-1/SKILL.md) |


### Application-Specific Skills
| Skill | Description | URL |
|-------|-------------|-----|
| `app` | Project overview, component navigation | [SKILL.md](context/skills/app/SKILL.md) |
| `app-ui` | Frontend patterns (Framework + UI Lib) | [SKILL.md](context/skills/app-ui/SKILL.md) |
| `app-pr` | Pull request conventions | [SKILL.md](context/skills/app-pr/SKILL.md) |


### AI Agent Skills
| `skill-creator` | Create new AI agent skills | [SKILL.md](context/skills/skill-creator/SKILL.md) |
| `skill-sync` | Sync skills to AI agent configuration | [SKILL.md](context/skills/skill-sync/SKILL.md) |


### Auto-invoke Skills

When performing these actions, ALWAYS invoke the corresponding skill FIRST:

#### Context: root (`.`)

| Action | Skill |
|--------|-------|
| After creating/modifying a skill | `skill-sync` |
| Create a PR with gh pr create | `app-pr` |
| Creating Yup schemas | `yup-1` |
| Creating new skills | `skill-creator` |
| Creating/modifying {Application} UI components | `app-ui` |
| Fill .github/pull_request_template.md (Context/Description/Steps to review/Checklist) | `app-pr` |
| General Ozzy development questions | `app` |
| Inspect PR CI workflows (.github/workflows/*): conventional-commit, pr-check-changelog, pr-conflict-checker, labeler | `app-pr` |
| Regenerate AGENTS.md Auto-invoke tables (sync.sh) | `skill-sync` |
| Review PR requirements: template, title conventions, changelog gate | `app-pr` |
| Troubleshoot why a skill is missing from AGENTS.md auto-invoke | `skill-sync` |
| Understand review ownership with CODEOWNERS | `app-pr` |
| Using Zustand stores | `zustand-5` |
| Working on {Application} UI structure (actions/adapters/types/hooks) | `app-ui` |
| Working with Tailwind classes | `tailwind-4` |
| Writing React components | `react-19` |
| Writing TypeScript types/interfaces | `typescript` |

#### Context: ui (`ui`)

| Action | Skill |
|--------|-------|
| Creating Yup schemas | `yup-1` |
| Creating/modifying {Application} UI components | `app-ui` |
| Using Zustand stores | `zustand-5` |
| Working on {Application} UI structure (actions/adapters/types/hooks) | `app-ui` |
| Working with Tailwind classes | `tailwind-4` |
| Writing React components | `react-19` |
| Writing TypeScript types/interfaces | `typescript` |


---
---
---
---
---
---
---

## Project Overview

{Application Name} is a {Description of Application}.

| Component | Location | Tech Stack |
|-----------|----------|------------|
| API | `api/` | {Backend Stack} |
| UI | `ui/` | {Frontend Stack} |

---

## Commit & Pull Request Guidelines

Follow conventional-commit style: `<type>[scope]: <description>`

**Types:** `feat`, `fix`, `docs`, `chore`, `perf`, `refactor`, `style`, `test`

Before creating a PR:
1. Complete checklist in `.github/pull_request_template.md`
2. Run all relevant tests and linters
3. Link screenshots for UI changes
