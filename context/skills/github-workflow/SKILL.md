---
name: github-workflow
description: >
  GitHub workflow best practices for teams. Trigger: When setting up repository workflows, branch strategies, or team collaboration.
license: Apache-2.0
metadata:
  author: author-name
  version: "1.0"
  scope: [root, ui, api]
  auto_invoke:
    - "Setting up GitHub workflow"
    - "Creating branch strategies"
    - "Configuring team collaboration"
    - "Setting up fork workflows"
allowed-tools: Read, Edit, Write, Glob, Grep, Bash, WebFetch, WebSearch, Task
---

## When to Use

Apply GitHub workflow practices when:
- Setting up new repositories
- Configuring team collaboration workflows
- Establishing branch strategies
- Setting up CI/CD pipelines
- Managing fork-based contributions
- Configuring code review processes

## Critical Patterns

### Fork-Based Workflow Architecture

**Repository Structure:**
- **upstream**: Original repository (read-only for contributors)
- **origin**: Your personal fork (read-write)
- **main/develop**: Primary branches (NEVER push directly)

**✅ Setup Commands:**
```bash
# Clone your fork
git clone git@github.com:your-username/repo.git
cd repo

# Add upstream as remote
git remote add upstream git@github.com:original-org/repo.git

# Verify remotes
git remote -v
# origin    git@github.com:your-username/repo.git (fetch)
# origin    git@github.com:your-username/repo.git (push)
# upstream  git@github.com:original-org/repo.git (fetch)
# upstream  git@github.com:original-org/repo.git (push)
```

### Branch Strategy

**Main Branches:**
- `main`: Production-ready code (protected)
- `develop`: Integration branch (next release)
- `feature/*`: New features (from origin)
- `hotfix/*`: Emergency fixes (from origin)

**✅ Feature Branch Workflow:**
```bash
# Sync with upstream before starting
git fetch upstream
git checkout develop
git rebase upstream/develop

# Create feature branch
git checkout -b feature/new-feature

# Work on feature...
git add .
git commit -m "feat: add new feature"

# Push to YOUR fork (origin)
git push origin feature/new-feature

# Create PR from your fork to upstream
```

### Daily Workflow Commands

**Sync with Upstream:**
```bash
# Update local develop with latest changes
git fetch upstream
git checkout develop
git rebase upstream/develop

# Push updated develop to your fork
git push origin develop
```

**Before Creating PR:**
```bash
# Ensure feature is up-to-date
git checkout feature/new-feature
git fetch upstream
git rebase upstream/develop

# Run tests and linting
npm run test
npm run lint

# Push to origin
git push origin feature/new-feature --force-with-lease
```

## Code Examples

### GitHub Actions Workflow (.github/workflows/ci.yml)
```yaml
name: CI Pipeline

on:
  pull_request:
    branches: [main, develop]
  push:
    branches: [main, develop]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0  # Full history for better analysis
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run tests
        run: npm run test:coverage
      
      - name: Run linting
        run: npm run lint
      
      - name: Upload coverage
        uses: codecov/codecov-action@v3
        with:
          file: ./coverage/lcov.info

  build:
    needs: test
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Build application
        run: npm run build
      
      - name: Upload build artifacts
        uses: actions/upload-artifact@v3
        with:
          name: build-files
          path: dist/
```

### Branch Protection Rules
```bash
# Set up branch protection via GitHub CLI
gh api repos/:owner/:repo/branches/main/protection \
  --method PUT \
  --field required_status_checks='{"strict":true,"contexts":["ci"]}' \
  --field enforce_admins=true \
  --field required_pull_request_reviews='{"required_approving_review_count":2}' \
  --field restrictions=null
```

### Team Collaboration Scripts

**setup-repo.sh** - For new team members:
```bash
#!/bin/bash
REPO_NAME=$1
ORG_NAME=$2

if [ -z "$REPO_NAME" ] || [ -z "$ORG_NAME" ]; then
  echo "Usage: ./setup-repo.sh <repo-name> <org-name>"
  exit 1
fi

# Clone fork
git clone git@github.com:$USER/$REPO_NAME.git
cd $REPO_NAME

# Add upstream
git remote add upstream git@github.com:$ORG_NAME/$REPO_NAME.git

# Set up branches
git checkout -b develop
git push origin develop

echo "✅ Repository setup complete!"
echo "Upstream: $ORG_NAME/$REPO_NAME"
echo "Origin: $USER/$REPO_NAME"
```

**sync-upstream.sh** - Daily sync script:
```bash
#!/bin/bash
# Sync develop branch with upstream

echo "🔄 Syncing with upstream..."

# Fetch latest changes
git fetch upstream

# Switch to develop
git checkout develop

# Rebase with upstream develop
git rebase upstream/develop

# Push to origin
git push origin develop

echo "✅ Sync complete!"
```

## Commands

### Repository Setup
```bash
# Fork repository (via GitHub CLI)
gh repo fork original-org/repo --clone=true

# Add upstream remote
git remote add upstream git@github.com:original-org/repo.git

# Set default branch to develop
git branch --set-upstream-to=origin/develop develop
```

### Daily Workflow
```bash
# Start new feature
git checkout develop
git pull origin develop
git checkout -b feature/amazing-feature

# During development
git add .
git commit -m "feat: implement amazing feature"
git push origin feature/amazing-feature

# Sync with upstream before PR
git fetch upstream
git rebase upstream/develop
git push origin feature/amazing-feature --force-with-lease
```

### Maintenance
```bash
# Clean up merged branches
git branch --merged | grep -v "main\|develop" | xargs git branch -d

# Update all remotes
git remote update

# Check repository status
git status --porcelain --branch
```

## Resources

- **Templates**: See [assets/](assets/) for workflow templates
- **Documentation**: See [references/](references/) for GitHub guides

---

## Best Practices Summary

### ✅ Do
- Use fork-based workflow for contributions
- Never push directly to main/develop
- Create feature branches from develop
- Keep branches up-to-date with upstream
- Use descriptive commit messages
- Create PRs with clear descriptions
- Require code reviews before merge

### ❌ Don't
- Push directly to main branch
- Work on main/develop directly
- Let feature branches diverge too much
- Use force push without --force-with-lease
- Merge without resolving conflicts first
- Skip tests before creating PR

### 🔄 Daily Routine
1. `git fetch upstream` - Get latest changes
2. `git rebase upstream/develop` - Update your work
3. `git push origin develop` - Share updates with team
4. Create PR when feature is ready
5. Request reviews from team members
