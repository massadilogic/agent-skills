# Repository Guidelines

## Purpose

This file acts as a lightweight router to the knowledge stored in context/skills.

- It does not define rules itself, only points to skills that contain detailed guidance.
- All project-specific knowledge should live inside context/skills.

## How to Use

- Treat this file as an entry point, not as documentation.
- When you need guidance, locate and invoke the appropriate skill.
- If a skill does not exist, create a new one inside context/skills.

## Skills Location

All skills are stored in the `context/skills` directory.
Each skill should be:

- Focused on a single concern
- Reusable
- Self-contained

## Available Skills
Below are examples of avalable skills and examples of how new skills may be organized.

### Core / Generic Skills (Recommended)

| Skill | Description | URL |
|-------|-------------|-----|
| `clean-code-principles` | Clean code principles for software design | [SKILL.md](contxt/skills/solid-principles/) |
| `solid-principles` | SOLID principles for software design | [SKILL.md](context/skills/solid-principles/SKILL.md) |
| `github-workflow` | GitHub workflow best practices for teams | [SKILL.md](context/skills/github-workflow/SKILL.md) |


### Project-Specific Skills (Examples)

After creating new skill it should be added to this section
(replace the examples with de real project skills)

Use the following format for project-specific skills:
- `{project}-architecture` - System design and structure (example: `app-architecture`)
- `{project}-ui` - Frontend patterns (Framework + UI Lib) (example: `app-ui`)



### AI Agent Skills
| `skill-creator` | Create new AI agent skills | [SKILL.md](context/skills/skill-creator/SKILL.md) |
| `skill-sync` | Sync skills to AI agent configuration | [SKILL.md](context/skills/skill-sync/SKILL.md) |


### Auto-invoke Skills

When performing these actions, ALWAYS invoke the corresponding skill FIRST:

| Action | Skill |
|--------|-------|
| Creating or modifying a skill | skill-creator |
| Updating skill registry / references | skill-sync |
| Applying design principles | solid-principles |
| Applying design principles | clean-code-principles |
| Working with version control workflows | github-workflow |
| Following project conventions | {project}-*

When a skill is created, it should be added to this section.


#### Invocation rules examples

| Action | Skill |
|--------|-------|
| Applying SOLID principles | `solid-principles` |
| Setting up GitHub workflow | `github-workflow` |

