# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This repository (`dascentral/reusable-workflows`) is a collection of reusable GitHub Actions workflows and composite actions consumed by other repositories via `workflow_call`. There is no application code, build step, or test suite here — the only files are YAML workflow/action definitions.

## Repository Structure

- `.github/workflows/` — Reusable workflows (called from other repos)
- `.github/actions/` — Composite actions (used within workflows)

### Reusable Workflows

| Workflow                | Purpose                                                         |
| ----------------------- | --------------------------------------------------------------- |
| `laravel-tests.yml`     | Run Laravel test suite with MySQL service container             |
| `laravel-lint.yml`      | Run Pint (PHP) + Prettier/ESLint (JS) with optional auto-commit |
| `laravel-analyze.yml`   | Run PHPStan static analysis                                     |
| `laravel-deploy.yml`    | Build and deploy Laravel app via rsync over SSH                 |
| `eslint.yml`            | Run ESLint with auto-commit                                     |
| `prettier.yml`          | Run Prettier with auto-commit                                   |
| `phpstan.yml`           | Run PHPStan (standalone, not Laravel-specific)                  |
| `branch-validation.yml` | Enforce branch naming conventions (e.g. `feature/`, `fix/`)     |

### Composite Actions

| Action          | Purpose                                                                |
| --------------- | ---------------------------------------------------------------------- |
| `laravel-setup` | Set up PHP, Node.js, and install Composer/npm dependencies for Laravel |
| `ssh-setup`     | Configure SSH agent and known hosts for deployment                     |

## Conventions

- All workflows use `workflow_call` trigger — they are never run directly in this repo.
- Most workflows skip `dependabot[bot]` via `if: github.actor != 'dependabot[bot]'`.
- Laravel workflows expect a `.env.ci` file in the calling repository.
- Private Composer packages are supported via the `composer-auth` secret.
- Several workflows use `stefanzweifel/git-auto-commit-action@v5` to commit formatting fixes.
- Composite actions are referenced by tag (e.g., `dascentral/reusable-workflows/.github/actions/laravel-setup@v0.4.0`).

## Versioning

Consumers pin to git tags (e.g., `@v0.4.0`). When making changes, a new tag must be created and consuming repos updated to reference it.
