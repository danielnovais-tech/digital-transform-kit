# Repository Automation and Quality Tools

This document describes the automated quality and hygiene tools configured for this repository.

## 📋 Table of Contents

- [Overview](#overview)
- [Contribution Guidelines](#contribution-guidelines)
- [Pull Request Process](#pull-request-process)
- [Automated Workflows](#automated-workflows)
- [Code Review Tools](#code-review-tools)
- [Labels and Categorization](#labels-and-categorization)
- [Dependency Management](#dependency-management)

## Overview

This repository includes comprehensive automation to maintain code quality, security, and consistency. All checks run automatically on pull requests.

## Contribution Guidelines

Full contribution guidelines are available in [CONTRIBUTING.md](../CONTRIBUTING.md) (in Portuguese). Key points:

- **Commit Standards**: Use [Conventional Commits](https://www.conventionalcommits.org/)
- **Branching**: Follow the branch naming conventions (`feature/*`, `bugfix/*`, etc.)
- **PR Template**: Fill out all sections of the PR template
- **Reviews**: At least one approval required for merging

## Pull Request Process

### 1. Creating a Pull Request

When you open a PR, use the provided template which includes:
- **Description**: What changed and why
- **Checklist**: Pre-merge requirements
- **Related Issue**: Links to associated issues
- **Impact Assessment**: Areas affected by changes

### 2. Automated Checks

All PRs trigger automated workflows:

```
PR Opened
    ├── Auto-labeling (instant)
    ├── Markdown Lint (if .md files changed)
    ├── CI Checks (conditional based on project type)
    └── CodeQL Security Scan
```

### 3. Review and Approval

- Automated checks must pass
- At least 1 maintainer approval required
- All conversations must be resolved

## Automated Workflows

### CI Workflow (`.github/workflows/ci.yml`)

**Triggers**: Pull requests and pushes to `main` or `develop`

**What it does**:
1. **Detects Package Managers**: Automatically identifies project type (Node.js, Python, Go)
2. **Conditional Checks**: Runs appropriate linters and tests based on detected technologies
3. **General Checks**: Always runs basic file validation

**Jobs**:
- `detect-package-managers`: Identifies project technologies
- `node-checks`: Runs npm install, lint, test, build (if package.json exists)
- `python-checks`: Runs pip install, flake8, pytest (if Python files exist)
- `go-checks`: Runs go build and test (if go.mod exists)
- `general-checks`: Validates YAML files and checks for trailing whitespace

**Permissions**: `contents: read`, `pull-requests: read`

### Markdown Lint Workflow (`.github/workflows/markdown-lint.yml`)

**Triggers**: Pull requests and pushes affecting `*.md` files

**What it does**:
1. **Markdown Linting**: Validates markdown syntax and formatting using markdownlint-cli2
2. **Link Checking**: Verifies all links in markdown files are valid

**Configuration**:
- Line length limit: 120 characters (`.markdownlint.json`)
- Link timeout: 20 seconds with retry on 429 (rate limiting)

**Permissions**: `contents: read`, `pull-requests: read`

### CodeQL Security Analysis (`.github/workflows/codeql.yml`)

**Triggers**:
- Pull requests to `main` or `develop`
- Pushes to `main` or `develop`
- Weekly schedule (Mondays at 00:00 UTC)

**What it does**:
- Scans code for security vulnerabilities
- Analyzes JavaScript and Python code
- Reports findings to Security tab

**Languages**: JavaScript, Python (automatically detected)

**Permissions**: `actions: read`, `contents: read`, `security-events: write`

### Auto-Label Workflow (`.github/workflows/auto-label.yml`)

**Triggers**: PR opened, edited, or reopened

**What it does**:
1. **Content-Based Labeling**: Analyzes PR title and body for keywords
2. **Size Labeling**: Calculates PR size based on changes (XS/S/M/L/XL)

**Label Mappings**:
- `enhancement`: feat, feature, add, implement, enhance, new
- `bugfix`: fix, bug, resolve, correct, patch, repair
- `documentation`: docs, documentation, readme, guide, comment
- `refactor`: refactor, cleanup, restructure, optimize, improve
- `dependencies`: dependency, dependencies, dependabot, upgrade
- `ci`: ci, workflow, github actions, pipeline, automation
- `security`: security, vulnerability, cve, exploit
- `breaking-change`: breaking change, breaking, major version

**Size Thresholds**:
- `size/XS`: < 10 changes
- `size/S`: 10-49 changes
- `size/M`: 50-199 changes
- `size/L`: 200-499 changes
- `size/XL`: 500+ changes

**Permissions**: `contents: read`, `pull-requests: write`, `issues: write`

## Code Review Tools

### CodeQL

- **Purpose**: Automated security vulnerability detection
- **Languages**: JavaScript, Python
- **Frequency**: Every PR + weekly scans
- **Results**: Available in the Security tab

### Manual Code Review

- Required for all PRs
- At least 1 approval needed
- Focus on logic, design, and maintainability

## Labels and Categorization

### Automatic Labels

Applied by the auto-label workflow based on PR content:

| Label | Description | Keywords |
|-------|-------------|----------|
| `enhancement` | New features or improvements | feat, feature, add, implement |
| `bugfix` | Bug fixes | fix, bug, resolve, correct |
| `documentation` | Documentation changes | docs, documentation, readme |
| `refactor` | Code refactoring | refactor, cleanup, restructure |
| `dependencies` | Dependency updates | dependency, dependabot, upgrade |
| `ci` | CI/CD changes | ci, workflow, github actions |
| `security` | Security-related changes | security, vulnerability, cve |
| `breaking-change` | Breaking changes | breaking change, major version |
| `size/XS` to `size/XL` | PR size indicator | Automatically calculated |

### Manual Labels

These should be added manually when needed:

- `priority:high` - Urgent issues
- `priority:medium` - Important but not urgent
- `priority:low` - Nice to have
- `needs-triage` - Requires initial review
- `good-first-issue` - Good for new contributors
- `help-wanted` - Looking for contributors

## Dependency Management

### Dependabot Configuration (`.github/dependabot.yml`)

**What it does**:
- Automatically checks for dependency updates
- Creates PRs for outdated dependencies
- Supports multiple package ecosystems

**Ecosystems Monitored**:
1. **GitHub Actions** - Workflow dependencies
2. **npm** - JavaScript/Node.js packages
3. **pip** - Python packages
4. **Docker** - Container images
5. **Go modules** - Go dependencies

**Schedule**: Weekly on Mondays at 09:00 UTC

**Limits**:
- GitHub Actions: Max 5 open PRs
- npm/pip/Go: Max 10 open PRs
- Docker: Max 5 open PRs

**Strategy**:
- Major version updates are ignored by default (can be manually updated)
- Uses `increase` versioning strategy for npm
- All updates labeled with `dependencies` + ecosystem-specific label
- Commit messages follow Conventional Commits format

**Commit Message Format**:
```
chore(deps): update <dependency> from X.Y.Z to X.Y.Z
```

## Configuration Files

### `.markdownlint.json`

Configures markdown linting rules:
- Line length: 120 characters (warning)
- Allows HTML in markdown
- Allows duplicate headings if they're siblings

### `.github/yamllint-config.yml`

Configures YAML linting:
- Line length: 120 characters (warning)
- Disables document-start requirement
- Allows less strict truthy checking

### `.github/markdown-link-check-config.json`

Configures link checking:
- Timeout: 20 seconds
- Retry on 429 (rate limiting)
- Retry count: 3
- Fallback delay: 30 seconds

## Best Practices

### For Contributors

1. **Before Opening a PR**:
   - Ensure all automated checks pass locally where possible
   - Follow the commit message format
   - Fill out the PR template completely

2. **During Review**:
   - Respond to review comments promptly
   - Make requested changes as separate commits
   - Keep the PR focused on a single concern

3. **After Approval**:
   - Ensure all checks still pass
   - Resolve any merge conflicts
   - Squash commits if requested

### For Maintainers

1. **Reviewing PRs**:
   - Check that automated checks passed
   - Verify the PR follows guidelines
   - Test changes locally if needed
   - Provide constructive feedback

2. **Merging PRs**:
   - Ensure all required approvals are present
   - Verify no merge conflicts
   - Use appropriate merge strategy (squash, merge, or rebase)

## Troubleshooting

### Common Issues

**Problem**: Markdown lint fails
- **Solution**: Check `.markdownlint.json` for rules, ensure line length < 120 chars

**Problem**: Auto-labeling doesn't work
- **Solution**: Ensure PR title or body contains recognized keywords

**Problem**: CI checks are skipped
- **Solution**: CI only runs when relevant package manager files exist (package.json, requirements.txt, go.mod)

**Problem**: CodeQL fails
- **Solution**: Check Security tab for details, may indicate a real security issue

### Getting Help

- Open an issue with the `question` label
- Check existing issues for similar problems
- Review [CONTRIBUTING.md](../CONTRIBUTING.md) for guidelines

## Future Enhancements

Potential additions (not yet implemented):

- **Codecov**: Code coverage reporting (requires test infrastructure)
- **SonarCloud**: Advanced code quality analysis (requires external setup)
- **Semantic Release**: Automated versioning and changelog generation
- **Conventional PR**: Validate PR titles follow Conventional Commits

---

*Last updated: January 2026*
