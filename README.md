# go-starter-repo

A personal starter repository for new Go projects.

This repo provides a consistent baseline for:
- tooling (`mise`)
- task orchestration (`just`)
- shell script conventions
- git and GitHub workflow
- quality checks (formatting, vetting, static analysis, vuln scanning)

It is intentionally opinionated and optimized for a solo developer workflow.

---

## What This Repo Includes

### Tooling
- `mise.toml` defines all required tools and versions
- ensures reproducible local environments

### Command Interface
- `justfile` exposes all common workflows
- wraps scripts for discoverability and consistency

### Script Foundation (`scripts/lib`)
- `lib.sh` → logging, guards, shared helpers
- `git.sh` → git state invariants (clean tree, branch checks)
- `paths.sh` → repo-local paths and conventions

### Workflow Scripts (`scripts/`)
- `test` → run tests (unit, race, coverage)
- `tidy` → `go mod tidy` + formatting
- `audit` → full quality gate:
  - `gofmt` check
  - `go mod tidy` diff check
  - `go mod verify`
  - `go vet`
  - `staticcheck`
  - `govulncheck`
- `mod-upgrade` → dependency upgrade report
- `confirm` → interactive safety prompt

### Git Workflow (`scripts/git/`)
A complete branch + PR lifecycle:

- branch management:
  - `branch-create`
  - `branch-delete`

- PR workflow:
  - `pr-create`
  - `pr-merge`
  - `pr-view`

- syncing:
  - `sync-main`
  - `sync-branch`
  - `rebase-main`
  - `rebase-upstream`

- recovery:
  - `repair-main`

These scripts enforce:
- clean working tree
- branch naming conventions
- upstream tracking
- confirmation for destructive/history-rewriting operations
- audit/test gates before publishing

---

## What This Repo Does *Not* Include

- no application structure (`cmd/`, `internal/`, etc.)
- no build or run commands
- no framework assumptions
- no database, templating, or runtime tooling

Those belong to the project layer.

---

## Usage

### 1. Clone the starter

```bash
git clone git@github.com:spcameron/go-starter-repo.git <app>
cd <app>
```

### 2. Remove starter history

```bash
rm -rf .git
```

### 3. Initialize the project

```bash
./scripts/init <app> <module>
```

Example:

```bash
./scripts/init scribe github.com/spcameron/scribe
```

This will:
- set the app name in `.project.toml`
- recreate `go.mod`
- reset `README.md`

---

### 4. Install tooling and verify

```bash
mise install
just test
```

---

### 5. Initialize git and create repo

```bash
git init
git add .
git commit -m "Initial commit from starter"
```

Create and push with GitHub CLI:

```bash
gh repo create <app> --source=. --remote=origin --push
```

---

## Common Commands

```bash
just test
just tidy
just audit
just maintain
```

Git workflow:

```bash
just branch-create
just pr-create
just pr-merge
just sync-branch
```

---

## Design Principles

- **Consistency over flexibility**  
  Encode one good workflow instead of supporting many.

- **Scripts over repetition**  
  Centralize behavior so improvements propagate.

- **Explicit safety**  
  Require confirmation for destructive or history-rewriting operations.

- **Separation of concerns**  
  Starter defines tooling and workflow—not application structure.

---

## Status

This is an evolving personal starter.

It is expected to change as patterns stabilize across projects.
